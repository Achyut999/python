from PyQt5.QtCore import * 
from PyQt5.QtWidgets import * 
from PyQt5.QtGui import * 
from PyQt5.QtWebEngineWidgets import * 
from PyQt5.QtPrintSupport import * 
import os
import requests
import shutil
import sys
class MainWindow(QMainWindow):
    def __init__(self, *args, **kwargs):
        super(MainWindow, self).__init__(*args, **kwargs)
        self.browser = QWebEngineView()
        self.browser.setUrl(QUrl("http://google.com")) ##keep new webpage later 
        self.browser.urlChanged.connect(self.update_urlbar)
        self.urlbar = QLineEdit()
        self.urlbar.returnPressed.connect(self.navigate_to_url)
        navtb.addWidget(self.urlbar)
        self.browser.loadFinished.connect(self.update_title)
        self.setCentralWidget(self.browser)
        self.status = QStatusBar()
        self.setStatusBar(self.status)
        navtb = QToolBar("Navigation")
        self.addToolBar(navtb)
        back_btn = QAction("Back", self)
        back_btn.setStatusTip("Back to previous page")
        back_btn.triggered.connect(self.browser.back)
        navtb.addAction(back_btn)
        next_btn = QAction("Forward", self)
        next_btn.setStatusTip("Forward to next page")
        next_btn.triggered.connect(self.browser.forward)
        navtb.addAction(next_btn)
        reload_btn = QAction("Reload", self)
        reload_btn.setStatusTip("Reload page")
        reload_btn.triggered.connect(self.browser.reload)
        navtb.addAction(reload_btn)
        home_btn = QAction("Home", self)
        home_btn.setStatusTip("Go home")
        home_btn.triggered.connect(self.navigate_home)
        navtb.addAction(ai_btn)
        navtb.addSeparator()
        self.urlbar = QLineEdit()
        home_btn.setStatusTip("Go home")
        home_btn.triggered.connect(self.navigate_ai)
        
        navtb.addAction(home_btn)
        navtb.addSeparator()
        self.urlbar = QLineEdit()
        home_btn.setStatusTip("Go home")
        home_btn.triggered.connect(self.navigate_home)
        
        stop_btn = QAction("Stop", self)
        stop_btn.setStatusTip("Stop loading current page")
        stop_btn.triggered.connect(self.browser.stop)
        navtb.addAction(stop_btn)
        self.show()
        self.bookmark_bar = QToolBar('Bookmark')
        self.bookmark_bar.setIconSize(QSize(12, 12))
        self.bookmark_bar.setMovable(False)
        self.addToolBar(self.bookmark_bar)
        class BookMarkToolBar(QtWidgets.QToolBar):
    bookmarkClicked = QtCore.pyqtSignal(QtCore.QUrl, str)

    def __init__(self, parent=None):
        super(BookMarkToolBar, self).__init__(parent)
        self.actionTriggered.connect(self.onActionTriggered)
        self.bookmark_list = []

    def setBoorkMarks(self, bookmarks):
        for bookmark in bookmarks:
            self.addBookMarkAction(bookmark["title"], bookmark["url"])

    def addBookMarkAction(self, title, url):
        bookmark = {"title": title, "url": url}
        fm = QtGui.QFontMetrics(self.font())
        if bookmark not in self.bookmark_list:
            text = fm.elidedText(title, QtCore.Qt.ElideRight, 150)
            action = self.addAction(text)
            action.setData(bookmark)
            self.bookmark_list.append(bookmark)

    @QtCore.pyqtSlot(QtWidgets.QAction)
    def onActionTriggered(self, action):
        bookmark = action.data()
        self.bookmarkClicked.emit(bookmark["url"], bookmark["title"])


class MainWindow(QtWidgets.QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.defaultUrl = QtCore.QUrl()

        self.tabs = QtWidgets.QTabWidget()
        self.tabs.setTabsClosable(True)
        self.setCentralWidget(self.tabs)

        self.urlLe = QtWidgets.QLineEdit()
        self.urlLe.returnPressed.connect(self.onReturnPressed)
        self.favoriteButton = QtWidgets.QToolButton()
        self.favoriteButton.setIcon(QtGui.QIcon("images/star.png"))
        self.favoriteButton.clicked.connect(self.addFavoriteClicked)

        toolbar = self.addToolBar("")
        toolbar.addWidget(self.urlLe)
        toolbar.addWidget(self.favoriteButton)

        self.addToolBarBreak()
        self.bookmarkToolbar = BookMarkToolBar()
        self.bookmarkToolbar.bookmarkClicked.connect(self.add_new_tab)
        self.addToolBar(self.bookmarkToolbar)
        self.readSettings()

    def onReturnPressed(self):
        self.tabs.currentWidget().setUrl(QtCore.QUrl.fromUserInput(self.urlLe.text()))

    def addFavoriteClicked(self):
        loop = QtCore.QEventLoop()

        def callback(resp):
            setattr(self, "title", resp)
            loop.quit()

        web_browser = self.tabs.currentWidget()
        web_browser.page().runJavaScript("(function() { return document.title;})();", callback)
        url = web_browser.url()
        loop.exec_()
        self.bookmarkToolbar.addBookMarkAction(getattr(self, "title"), url)

    def add_new_tab(self, qurl=QtCore.QUrl(), label='Blank'):
        web_browser = QtWebEngineWidgets.QWebEngineView()
        web_browser.setUrl(qurl)
        web_browser.adjustSize()
        web_browser.urlChanged.connect(self.updateUlrLe)
        index = self.tabs.addTab(web_browser, label)
        self.tabs.setCurrentIndex(index)
        self.urlLe.setText(qurl.toString())

    def updateUlrLe(self, url):
        self.urlLe.setText(url.toDisplayString())

    def readSettings(self):
        setting = QtCore.QSettings()
        self.defaultUrl = setting.value("defaultUrl", QtCore.QUrl('http://www.google.com'))
        self.add_new_tab(self.defaultUrl, 'Home Page')
        self.bookmarkToolbar.setBoorkMarks(setting.value("bookmarks", []))

    def saveSettins(self):
        settings = QtCore.QSettings()
        settings.setValue("defaultUrl", self.defaultUrl)
        settings.setValue("bookmarks", self.bookmarkToolbar.bookmark_list)

    def closeEvent(self, event):
        self.saveSettins()
        super(MainWindow, self).closeEvent(event)


if __name__ == '__main__':
    import sys

    QtCore.QCoreApplication.setOrganizationName("eyllanesc.org")
    QtCore.QCoreApplication.setOrganizationDomain("www.eyllanesc.com")
    QtCore.QCoreApplication.setApplicationName("MyApp")
    app = QtWidgets.QApplication(sys.argv)
    w = MainWindow()
    w.show()
    sys.exit(app.exec_())
def download(url):
    filename = url.split("/")[-1]
    path = "downloads/" + filename
    proxies = {
      'http': 'http://10.10.1.10:3128',
      'https': 'http://10.10.1.10:1080',
    }
    r = requests.get(url, stream=True, proxies=proxies)
    if r.status_code == 200:
        with open(path, 'wb') as f:
            r.raw.decode_content = True
            shutil.copyfileobj(r.raw, f)
    else:
        r.raise_for_status()
        PLUGIN_FOLDER = "plugins"

for filename in os.listdir(PLUGIN_FOLDER):
    if filename.endswith(".py") and filename != "__init__.py":
        module_name = filename[:-3]  # remove .py
        module_path = f"{PLUGIN_FOLDER}.{module_name}"
        plugin = importlib.import_module(module_path)

        if hasattr(plugin, "register"):
            plugin.register()
        if hasattr(plugin, "run"):
            plugin.run()
    try:
    plugin.register()
except Exception as e:
    print(f"Plugin {module_name} failed to register: {e}")



download('http://test.com/somefile.avi')
    def update_title(self):
        title = self.browser.page().title()
        self.setWindowTitle("% s - Geek Browser" % title)
    def navigate_home(self):
        self.browser.setUrl(QUrl("http://www.google.com"))
    def navigate_to_url(self):
        q = QUrl(self.urlbar.text())
        if q.scheme() == "":
            q.setScheme("http")
        self.browser.setUrl(q)
    def update_urlbar(self, q):
        self.urlbar.setText(q.toString())
        self.urlbar.setCursorPosition(0)
    def navigate_ai(self):
        self.browser.setUrl(QUrl("https://chatgpt.com/?model=auto"))
app = QApplication(sys.argv)


app.setApplicationName("idk what to name this browser Browser")

window = MainWindow()

# loop
app.exec_()
