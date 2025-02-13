<div align=center>
  <h1>Tkinter-nav</h1>
  <h3>Navigation wrapper for Tkinter.</h3>
</div>

## This fork is currently supported!

## Install
### Using pip
```
pip install git+https://github.com/Aareon/tkinter-nav
```
### Using pipenv
```
pipenv install git+https://github.com/Aareon/tkinter-nav
```

## Usage

### Create Some Pages

```python
import tkinter as tk
import tkinter_nav as tknav


class PageOne(tknav.Page):

    def __init__(self, parent):
        tknav.Page.__init__(self, parent, 'name_of_page')

        # Use as any Frame
        # Page extends tk.Frame
        tk.Button(self, ...).pack()

    def page_did_mount(self):
      ...

    def page_did_unmount(self):
      ...


class PageTwo(tknav.Page):
  ...


class PageThree(tknav.Page):
  ...
```

### Create your App

```python
...

class App(tknav.Wrapper):

  def __init__(self):
    pages = [PageOne, PageTwo]

    tknav.Wrapper.__init__(
        self,
        # Your pages
        pages=pages,
        # Set inital state, not required
        start_state={'foo': 'bar'}
    )

    # Use as any Tk instance
    # tknav.Wrapper extends tk.Tk
    self.geometry('200x200')
    self.title('My Nav App')

    # Show page
    self.show_page('page_one')


# Run
if __main__ == '__main__':
  App().mainloop()
```

## Pages

### Navigate Between Pages

* From the **constructor**

```python
class PageOne(tknav.Page):

    def __init__(self, parent):
        ...

        tk.Button(
          command=lambda: self.navigate('page_two')
        ).pack()
```

* From a **handler** function

```python
class PageOne(tknav.Page):
    ...

    def handleClick(self):
      self.navigate('page_two')
```

### Mount and Unmount

* **page_did_mount**: When the page is shown
* **page_did_unmount**: When the page is hidden

> **Note:** You do not have to use them. They will be defined with a <u>pass</u> statement.

```python
class SomePage(tknav.Page):
    ...

    def page_did_mount(self):
      print('Page did mount')

    def page_did_unmount(self):
      print('Page did unmount')
```

> **Note:** If you are familiar with React, they share the same role as ComponentDidMount and ComponentDidUnmount.

### State

You can set a global state for your app which will enable you to share data between pages.

```python
class App(tknav.Nav):

  def __init__(self):
    ...

    self.app_state = {'foo': 'bar'}

class PageOne(tknav.Page):
    ...

    def function(self):
      # get a value
      print(self.app_state['foo']) #bar

      # set a new value
      self.app_state['bar'] = 'foo'
```

When navigate() is called, the local state is merged with the global state.

```python
class PageTwo(tknav.Page):
    ...

    def function(self):
      print(self.app_state['bar']) #foo
```

> **Note:** If you are familiar with React, same principle.

### Example

Is this [example](https://github.com/maxzaleski/tkinter-nav/blob/master/example/example.py), we navigated from *page_one* to *page_two* to *page_one* again. The state is printed in page_did_mount() and changed in page_did_unmount().

<img src='https://i.imgur.com/RkfIsT1.png' height=200/>
