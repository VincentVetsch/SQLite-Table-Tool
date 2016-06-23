from PyQt4 import QtCore, QtGui, QtSql
import timer

class createDB:
    def __init__(self, data, field):
        self.db = QtSql.QSqlDatabase.addDatabase('QSQLITE')
        self.db.setDatabaseName(data)
        self.fields = field
        self.create()
    @timer.timer
    def create(self):
        if not self.db.open():
            QtGui.QMessageBox.critical(None, QtGui.qApp.tr("Cannot open database"),
             QtGui.qApp.tr("Unable to establish a database connection.\n"
                "This example needs SQLite support. Please read "
                "the Qt SQL driver documentation for information "
                "how to build it.\n\n" "Click Cancel to exit."),
             QtGui.QMessageBox.Cancel)
            return False
        query = QtSql.QSqlQuery(self.db)
        query.exec_(self.fields)
        return True


class Ui_Form(QtGui.QWidget):
    def setupUi(self, Form):
        self.fieldInt = 0
        self.fieldList = []
        self.labellist = []
        self.lineEditlist = []
        self.nNList = [] #Not Null
        self.pkList = [] #Primary Key
        self.aiList = [] #Auto Increment
        self.uList = []  #Unique
        self.addButtList = []
        self.delButtList = []
        self.typeList = []
        self.types = ["TEXT", "INTEGER", "REAL", "BLOB", "NUMERIC"]
        self.widgetList = [self.labellist, self.lineEditlist, self.typeList, self.nNList,
                           self.pkList, self.aiList, self.uList, self.addButtList, self.delButtList]
        Form.setWindowTitle("Create SQLite Database")
        icon = QtGui.QIcon()
        icon.addPixmap(QtGui.QPixmap("Resources/images/Python.ico"), QtGui.QIcon.Normal, QtGui.QIcon.Off)
        Form.setWindowIcon(icon)
        Form.resize(750, 200)
        self.center()
        self.setLayout()
        self.add_field()
        self.actionBar()

    @timer.timer
    def actionBar(self):
        self.hlay = QtGui.QHBoxLayout()
        self.db = QtGui.QLineEdit()
        self.dblabel = QtGui.QLabel("Database Name")
        self.table = QtGui.QLineEdit()
        self.tableLabel = QtGui.QLabel("Table Name")
        self.createTable = QtGui.QPushButton("Create Table")
        self.close = QtGui.QPushButton("Close")
        actionbar = (self.db, self.dblabel, self.table, self.tableLabel, self.createTable, self.close)
        for i in actionbar: self.hlay.addWidget(i)
        self.createTable.clicked.connect(self.read_fields)
        self.close.clicked.connect(Form.close)
        self.vlay.addLayout(self.hlay)

    @timer.timer
    def setLayout(self):
        self.vlay = QtGui.QVBoxLayout(Form)
        self.scrollArea = QtGui.QScrollArea()
        self.vlay.addWidget(self.scrollArea)
        self.scrollArea.setLayoutDirection(QtCore.Qt.LeftToRight)
        self.scrollArea.setWidgetResizable(True)
        self.scrollAreaWidgetContents = QtGui.QWidget(self.scrollArea)
        self.verticalLayoutScroll = QtGui.QVBoxLayout(self.scrollAreaWidgetContents)

    def add_field(self):
        fid = self.fieldInt
        self.hlayout2 = QtGui.QHBoxLayout()
        self.verticalLayoutScroll.addLayout(self.hlayout2)
        self.verticalLayoutScroll.setAlignment(QtCore.Qt.AlignTop)
        self.labellist.append(QtGui.QLabel("Field Name: "))
        self.lineEditlist.append(QtGui.QLineEdit())
        self.typeList.append(QtGui.QComboBox())
        self.typeList[fid].addItems(self.types) 
        self.nNList.append(QtGui.QCheckBox("Not Null"))
        self.pkList.append(QtGui.QCheckBox("Pri Key"))
        self.aiList.append(QtGui.QCheckBox("Auto Inc"))
        self.uList.append(QtGui.QCheckBox("Unique"))
        self.addButtList.append(QtGui.QPushButton("Add"))
        self.delButtList.append(QtGui.QPushButton("Delete"))
        for w in self.widgetList: self.hlayout2.addWidget(w[-1])      
        self.addButtList[self.fieldInt].clicked.connect(self.add_field)
        self.delButtList[self.fieldInt].clicked.connect(lambda: self.del_field(fid))
        self.scrollArea.setWidget(self.scrollAreaWidgetContents)
        self.fieldInt += 1 

    def del_field(self, vall):
        for i in self.widgetList:
            i[vall].deleteLater()
            i[vall] = None
            self.hlayout2.removeWidget(i[vall])

    def read_fields(self):
        fd = QtGui.QFileDialog()
        dbname = fd.getSaveFileName(caption='Save file', 
                directory='\\' + self.db.text(), filter='*.db')
        query = 'CREATE TABLE ' + self.table.text() + '('
        for i in range(0, len(self.lineEditlist)):
            if self.lineEditlist[i] != None:
                query += self.lineEditlist[i].text() + ' '
                query += self.typeList[i].currentText()
                if self.nNList[i].isChecked(): query += ' NOT NULL'
                if self.pkList[i].isChecked(): query += ' PRIMARY KEY'
                if self.aiList[i].isChecked(): query += ' AUTOINCREMENT'
                if self.uList[i].isChecked(): query += ' UNIQUE'
                if i < (len(self.lineEditlist) - 1): 
                    query += ', '
        query += ')'
        try:
            createDB(dbname, str(query))
        except Exception as exc:
            import pdb; pdb.set_trace()  # breakpoint 35d07432x //
            Form.close()
    
    def center(self):
        """
        Center the window on screen. This implemention will handle the window
        being resized or the screen resolution changing.
        """
        screen = QtGui.QDesktopWidget().screenGeometry()
        mysize = self.geometry()
        hpos = ( screen.width() - mysize.width() ) / 2
        vpos = ( screen.height() - mysize.height() ) / 2
        self.move(hpos, vpos)


if __name__ == "__main__":
    import sys
    app = QtGui.QApplication(sys.argv)
    app.setStyle("plastique")
    Form = QtGui.QWidget()
    ui = Ui_Form()
    ui.setupUi(Form)
    Form.show()
    sys.exit(app.exec_())
