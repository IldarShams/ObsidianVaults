---
tags:
- python
- multiprocessing
- process
- qthread
- emmiter
---
---
### Задача
Есть задача наладить общение между двумя процессами. Один процесс выступает в роли клиента, который обращается другому процессу для обработки некоторых данных. Данные обрабатываются некоторое продолжительное время, поэтому клиент не имеет возможности ждать завершения обработки для получения данных.

### Проблема
Поскольку программа python способна исполнятся только в одном потоке, threading или qthread ( используется PyQT для gui) нам не подходит. Необходимо использовать библиотеку multiprocessing, для исполнения сервиса и клиента (gui) в разных процессах. 

Из процесса сервера нет возможности вызвать событие на стороне клиента, поэтому клиенту приходиться ждать, когда сервис выполнит обработку.

### Решение
Создание отдельного потока emmiter (QThread) на стороне клиента, который ждёт результатов обработки на стороне сервиса и вызывает соотвествующее событие, когда сервис закончил обработку.

### Код
Пример, пользователь вводит в поле текст, дочерний процесс переписывает его заглавными буквами.
```python
import sys  
from multiprocessing import Process, Queue, Pipe  
  
from PyQt5.QtCore import pyqtSignal, QThread  
from PyQt5.QtWidgets import QApplication, QLineEdit, QTextBrowser, QVBoxLayout, QDialog  
  
  
class Emitter(QThread):  
    """ Emitter waits for data from the capitalization process and emits a signal for the UI to update its text. """  
    ui_data_available = pyqtSignal(str)  # Signal indicating new UI data is available.  
  
    def __init__(self, from_process: Pipe):  
        super().__init__()  
        self.data_from_process = from_process  
  
    def run(self):  
        while True:  
            try:  
                text = self.data_from_process.recv()  
            except EOFError:  
                break  
            else:  
                self.ui_data_available.emit(text.decode('utf-8'))  
  
  
class ChildProc(Process):  
    """ Process to capitalize a received string and return this over the pipe. """  
  
    def __init__(self, to_emitter: Pipe, from_mother: Queue, daemon=True):  
        super().__init__()  
        self.daemon = daemon  
        self.to_emitter = to_emitter  
        self.data_from_mother = from_mother  
  
    def run(self):  
        """ Wait for a ui_data_available on the queue and send a capitalized version of the received string to the pipe. """  
        while True:  
            text = self.data_from_mother.get()  
            self.to_emitter.send(text.upper())  
  
  
class Form(QDialog):  
    def __init__(self, child_process_queue: Queue, emitter: Emitter):  
        super().__init__()  
        self.process_queue = child_process_queue  
        self.emitter = emitter  
        self.emitter.daemon = True  
        self.emitter.start()  
  
        # ------------------------------------------------------------------------------------------------------------  
        # Create the UI        # -------------------------------------------------------------------------------------------------------------        self.browser = QTextBrowser()  
        self.lineedit = QLineEdit('Type text and press <Enter>')  
        self.lineedit.selectAll()  
        layout = QVBoxLayout()  
        layout.addWidget(self.browser)  
        layout.addWidget(self.lineedit)  
        self.setLayout(layout)  
        self.lineedit.setFocus()  
        self.setWindowTitle('Upper')  
  
        # -------------------------------------------------------------------------------------------------------------  
        # Connect signals        # -------------------------------------------------------------------------------------------------------------        # When enter is pressed on the lineedit call self.to_child        self.lineedit.returnPressed.connect(self.to_child)  
  
        # When the emitter has data available for the UI call the updateUI function  
        self.emitter.ui_data_available.connect(self.updateUI)  
  
    def to_child(self):  
        """ Send the text of the lineedit to the process and clear the lineedit box. """  
        self.process_queue.put(self.lineedit.text().encode('utf-8'))  
        self.lineedit.clear()  
  
    def updateUI(self, text):  
        """ Add text to the lineedit box. """  
        self.browser.append(text)  
  
  
if __name__ == '__main__':  
    # Some setup for qt  
    app = QApplication(sys.argv)  
  
    # Create the communication lines.  
    mother_pipe, child_pipe = Pipe()  
    queue = Queue()  
  
    # Instantiate (i.e. create instances of) our classes.  
    emitter = Emitter(mother_pipe)  
    child_process = ChildProc(child_pipe, queue)  
    form = Form(queue, emitter)  
  
    # Start our process.  
    child_process.start()  
  
    # Show the qt GUI and wait for it to exit.  
    form.show()  
    app.exec_()
```





