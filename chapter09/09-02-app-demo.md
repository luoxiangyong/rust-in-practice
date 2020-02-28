## 第一个GTK程序



```toml
[package]
name = "gprl-gtk-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies.gtk]
version = "0.8.0"
#features = ["v3_16"]

[dependencies.gio]
version = ""
#features = ["v2_44"]
```

```rust
extern crate gtk;
extern crate gio;

use gtk::prelude::*;
use gio::prelude::*;

use gtk::{Application, ApplicationWindow, Button};

fn main() {
    let application = Application::new(
        Some("com.github.luoxiangyong.examples.gtk.demo"),
        Default::default(),
    ).expect("failed to initialize GTK application");

    application.connect_activate(|app| {
        let window = ApplicationWindow::new(app);
        window.set_title("第一个GTK程序");
        window.set_default_size(350, 70);

        let button = Button::new_with_label("点我啊！");
        button.connect_clicked(|_| {
            println!("哎呀，真的点了!");
        });
        window.add(&button);

        window.show_all();
    });

    application.run(&[]);
}
```

编译运行后的程序如下：

![gprl-gtk-demo](/home/luoxiangyong/writing-book/rust-in-practice/assets/gprl-gtk-demo.png)

### 闭包和回调

在GUI编程中，当有个时间发生时，回调一个函数是很常见的，下面看看在gtk-rs如何做。

闭包可以捕获调用她时的环境，这点与函数指针不同，这个特性很有用，我们不在需要传递参数了。但是闭包的生命周期很难跟踪。在gtk-rs中，我们确保闭包的生命周期是`static`的，这样的话捕获的对象在闭包被调用是依然是存活的。我们看个例子吧。

在C语言中，我们一般这么写：

```c
#include <gtk/gtk.h>

void callback_clicked(GtkWidget *widget, gpointer data) {
    gtk_button_set_label(GTK_BUTTON(widget), "Window");
}
```

而在Rust中，我们这么写：

```rust
use gtk::{Button, ButtonExt};

let button = Button::new_with_label("Click me!");
button.connect_clicked(|but| {
    but.set_label("I've been clicked!");
});
```

很简单不是吗？如果在按钮被点击的情况下更新另外的窗口呢？比如下面这样：

```rust
use gtk::{Box, Button, ButtonExt, ContainerExt, WidgetExt};

// 先创建一个布局 layout
let container = Box::new(gtk::Orientation::Vertical, 5);
// label的文字将在闭包中被修改
let label = gtk::Label::new("");
let button = Button::new_with_label("Click me!");
button.connect_clicked(move |_| {
    label.set_label("Button has been clicked!");
});

container.add(&button);
container.add(&label);

```

很不辛，编译这段代码，会出现如下错误：

```bash
error[E0382]: use of moved value: `label`
```

它是说`label`的所有权被转移到闭包后，又被12行的代码使用了！为了让这段代码正常工作，我们只需要在将`label`转移（`move`）到闭包前`clone`它就可以了：

```rust
use gtk::{Box, Button, ButtonExt, ContainerExt, WidgetExt};

let container = Box::new(gtk::Orientation::Vertical, 5);

let label = gtk::Label::new("");
let button = Button::new_with_label("Click me!");

let label_clone = label.clone(); // <-- 这里clone一下下啦！
button.connect_clicked(move |_| {
    label_clone.set_label("Button has been clicked!");
});
```

现在再编译就没问题了。记住，`clone`只是带来了拷贝指针的代价，这不是问题。

**在闭包中时候非gtk-rs对象**

举个栗子，假如你正在写一个多窗口的应用程序，想在程序中跟踪窗口，以便在多个闭包中访问他们，该如何做呢？

一种方法是使用标准库中的`Rc`和`RefCell`。我们看看怎么做：

```rust
use gtk::{Button, ButtonExt, Window};

use std::cell::RefCell;
use std::collections::HashMap;
use std::rc::Rc;

let windows: Rc<RefCell<HashMap<usize, Window>>> = Rc::new(RefCell::new(HashMap::new()));
let button = Button::new_with_label("Click me!");
// We copy the reference to the cell containing the hashmap.
let windows_clone = windows.clone();
button.connect_clicked(move |_| {
    // create_window functions creates a window and return the following tuple: (usize, Window).
    let (window_id, window) = create_window();
    windows_clone.borrow_mut().unwrap().insert(window_id, window);
});

 ...

another_button.connect_clicked(move |_| {
    let id_to_remove = get_id_to_remove();
    windows.borrow_mut().unwrap().remove(&id_to_remove);
});
```

下面解释下`Rc<RefCell<T>>`是如何工作的：

+ `Rc`就是一个引用计数器，它记录了包含的对象示例被引用的次数，当计数为零时释放该对象。

+ `RefCell`复杂一点，它可以让一个不可变(unmutable)对象变得可变(mutable)。

下面这个宏可以让生活轻松点，不过理解这段代码可能不太轻松，需要你理解Rust的语法：

```rust
macro_rules! clone {
    (@param _) => ( _ );
    (@param $x:ident) => ( $x );
    ($($n:ident),+ => move || $body:expr) => (
        {
            $( let $n = $n.clone(); )+
            move || $body
        }
    );
    ($($n:ident),+ => move |$($p:tt),+| $body:expr) => (
        {
            $( let $n = $n.clone(); )+
            move |$(clone!(@param $p),)+| $body
        }
    );
}
```

这段代码意思就是帮你在闭包前调用一要转移到闭包中变量的`clone`一下，因此，上面的代码就可以这样写了：

```rust
let windows: Rc<RefCell<HashMap<usize, Window>>> = Rc::new(RefCell::new(HashMap::new()));
button.connect_clicked(clone!(windows => move |_| {
    let (window_id, window) = create_window();
    windows.borrow_mut().unwrap().insert(window_id, window);
}));
```

### 向上或向下转型(Upcast and downcast)

因为GTK有一套继承体系，很自然gtk-rs中也有。一般情况下，多数人不需要了解这些，不过了解一下它是怎么工作的也未尝不可。

向上转型比较简单：

```rust
let button = gtk::Button::new_with_label("Click me!");
let widget = button.upcast::<gtk::Widget>();
```

因为`Button`结构实现了`IsA<Widget>`，我们可以将它向上转型为`Widget`。每个`Widget`及其父类都实现了`IsA`trait，都可以做类似的向上转型。

如果你想写一个通用函数用来检查某个`Widget`是不是`Box`，代码像这样：

```rust
fn is_a_box<W: IsA<gtk::Object> + IsA<gtk::Widget> + Clone>(widget: &W) -> bool {
    widget.clone().upcast::<gtk::Widget>().is::<gtk::Box>()
}
```

这段代码什么意思呢？首先，注意到我们使用了`IsA`trait。参数`widget`需要实现`IsA<Widget>` 和 `IsA<Object>`。其次，我们需要`Object`能够使用`Cast`trait（包含`upcast`和`downcast`方法）。

其实，我们并不需要参数`widget`一定要是`Widget`类型的，这里这么写是为了简单，容易理解。

上面这个函数的关键点是将`widget`向上转型到更高类型的Widget，然后向下转型到需要的对象类型。我们可以先让这个函数更通用，像这样：

```rust
fn is_a<W: IsA<gtk::Object> + IsA<gtk::Widget> + Clone,
        T: IsA<gtk::Object> + IsA<gtk::Widget>>(widget: &W) -> bool {
    widget.clone().upcast::<gtk::Widget>().downcast::<T>().is_ok()
}
```

然后这样使用它：

```rust
let button = gtk::Button::new_with_label("Click me!");

assert_eq!(is_a::<_, gtk::Container>(&button), true);
assert_eq!(is_a::<_, gtk::Label>(&button), false);
```

有了以上的解释，理解第一程序是不是就简单了？好了我们说说后面的主题。