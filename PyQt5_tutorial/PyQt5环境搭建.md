
# PyQt5 安装

安装PyQt5

```
pip install PyQt5
```

安装Qt-designer

```
pip install PyQt5-tools
```
mac 需要 手动 安装 qt creator

# 配置pycharm环境

## 安装 qt-designer 插件

![1](https://ws4.sinaimg.cn/large/a17dfad9ly1g2nquiltd9j20t20k2ac4.jpg)
Name：可自己定义    
Program：指向上述安装PyQt5-tools里面的designer.exe    
Work directory：使用变量 ```$FileDir$   ```

## 安装 PyUIC 插件

![2](https://ws3.sinaimg.cn/large/a17dfad9ly1g2nqui2ylpj20sa0hxdh6.jpg)
  
```
Arguments:-m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py
            mac :  $FileName$ -o $FileNameWithoutExtension$.py
Program：指向python解释器    
Work directory：使用变量$FileDir$
```

## 安装pyqrc

![3](https://ws4.sinaimg.cn/large/a17dfad9ly1g2nquice38j20ht0e1ab0.jpg)
```
Name: Pyrcc  
Program: $FileDir$\venv\Scripts\pyrcc5.exe  
Arguments: $FileName$ -o $FileNameWithoutExtension$_rc.py  
Working directory: $FileDir$  
```

## 测试运行代码


```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow
if __name__ == '__main__':
    app = QApplication(sys.argv)
    MainWindow = QMainWindow()
    ui = Ui_MainWindow()
    ui.setupUi(MainWindow)
    MainWindow.show()
    sys.exit(app.exec_())
```

# 程序打包

- 安装 pyinstaller:  
```
pip install pyinstaller
```
- 常用可选项及说明:  


        -F：打包后只生成单个exe格式文件；

        -D：默认选项，创建一个目录，包含exe文件以及大量依赖文件；

        -c：默认选项，使用控制台(就是类似cmd的黑框)；

        -w：不使用控制台；

        -p：添加搜索路径，让其找到对应的库；

        -i：改变生成程序的icon图标。
- 命令:  
```
pyinstaller -w -i asd.ico untitled.py
```

- 资源一起打包  
```
a = Analysis(
    ...
    datas = [('you/source/file/path','file_name_in_project'),
    ('source/file2', 'file_name2')]
    ...
    )
```
        可以认为datas是一个List,每个元素是一个二元组。元组的第一个元素是你本地文件索引，第二个元素是拷贝到项目中之后的文件名字。除了上面那种写法，也可以将其提出来。

# 基本运行框架


```python
## ui 布局文件(qt-designer自动生成)

from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(1035, 742)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.label = QtWidgets.QLabel(self.centralwidget)
        self.label.setGeometry(QtCore.QRect(130, 70, 121, 51))
        font = QtGui.QFont()
        font.setPointSize(14)
        self.label.setFont(font)
        self.label.setObjectName("label")
        self.pushButton = QtWidgets.QPushButton(self.centralwidget)
        self.pushButton.setGeometry(QtCore.QRect(120, 140, 121, 51))
        self.pushButton.setObjectName("pushButton")
        self.radioButton = QtWidgets.QRadioButton(self.centralwidget)
        self.radioButton.setGeometry(QtCore.QRect(360, 80, 111, 41))
        self.radioButton.setSizeIncrement(QtCore.QSize(0, 0))
        self.radioButton.setObjectName("radioButton")
        self.toolButton = QtWidgets.QToolButton(self.centralwidget)
        self.toolButton.setGeometry(QtCore.QRect(380, 160, 37, 18))
        self.toolButton.setObjectName("toolButton")
        self.lineEdit = QtWidgets.QLineEdit(self.centralwidget)
        self.lineEdit.setGeometry(QtCore.QRect(160, 240, 261, 381))
        self.lineEdit.setStyleSheet("border-image: url(:/image/20141207114432_8iBaj.png);")
        self.lineEdit.setObjectName("lineEdit")
        self.dial = QtWidgets.QDial(self.centralwidget)
        self.dial.setGeometry(QtCore.QRect(680, 370, 171, 161))
        self.dial.setObjectName("dial")
        self.lcdNumber = QtWidgets.QLCDNumber(self.centralwidget)
        self.lcdNumber.setGeometry(QtCore.QRect(670, 230, 201, 101))
        self.lcdNumber.setObjectName("lcdNumber")
        self.graphicsView = QtWidgets.QGraphicsView(self.centralwidget)
        self.graphicsView.setGeometry(QtCore.QRect(760, 40, 151, 151))
        self.graphicsView.setStyleSheet("border-image: url(:/image/game_128px_1227323_easyicon.net.ico);")
        self.graphicsView.setObjectName("graphicsView")
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 1035, 23))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        self.dial.valueChanged['int'].connect(MainWindow.click_test)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))
        self.label.setText(_translate("MainWindow", "变换我的文字"))
        self.pushButton.setText(_translate("MainWindow", "PushButton"))
        self.radioButton.setText(_translate("MainWindow", "RadioButton"))
        self.toolButton.setText(_translate("MainWindow", "..."))


import resource_rc

## 编辑功能类

# 导入uitestPyQt5.ui转换为uitestPyQt5.py中的类
from untitled import Ui_MainWindow

from PyQt5 import QtWidgets
from PyQt5.QtCore import pyqtSlot
import sys


class my_window(QtWidgets.QMainWindow, Ui_MainWindow):
    # 建立的是Main Window项目，故此处导入的是QMainWindow
    # 参考博客中建立的是Widget项目，因此哪里导入的是QWidget
    def __init__(self):
        self.i = 0
        super(my_window, self).__init__()
        self.setupUi(self)


'''
    # 这里填写槽函数
    # @pyqtSlot() 为装饰器,避免重名,参数可以根据qt-designer填
    
    @pyqtSlot()
    def click_test(self,g):
        self.lcdNumber.display(g)
'''

if __name__ == '__main__':
    app = QtWidgets.QApplication(sys.argv)
    window = my_window()
    window.show()
    sys.exit(app.exec_())
```
