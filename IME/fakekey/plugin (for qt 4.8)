#include <QDebug>
#include <QtDeclarative>
#include <QtDeclarative/QDeclarativeExtensionPlugin>

#include <QApplication>
#include <QDeclarativeView>

class FakekeyModel : public QObject
{
Q_OBJECT

public:
  FakekeyModel(QObject *parent=0) : QObject(parent) {
}

~FakekeyModel() {
}

Q_INVOKABLE int sendKey(const QString &msg) {

    QDeclarativeView * receiver = qobject_cast<QDeclarativeView *>(QApplication::focusObject());
    if (!receiver) { 
        qDebug() << "simulateKeyPressEvent(): GuiApplication::focusObject() is 0 or not a QQuickItem."; 
        return 1; 
    }

    if(msg.startsWith(":enter")){
        QKeyEvent pressEvent = QKeyEvent(QEvent::KeyPress, Qt::Key_Return, Qt::NoModifier);
        QKeyEvent releaseEvent = QKeyEvent(QEvent::KeyRelease, Qt::Key_Return, Qt::NoModifier);
        qApp->sendEvent(receiver, &pressEvent);
        qApp->sendEvent(receiver, &releaseEvent);
        return 0;
    }

    if(msg.startsWith(":backspace")){
        QKeyEvent pressEvent = QKeyEvent(QEvent::KeyPress, Qt::Key_Backspace, Qt::NoModifier);
        QKeyEvent releaseEvent = QKeyEvent(QEvent::KeyRelease, Qt::Key_Backspace, Qt::NoModifier);
        qApp->sendEvent(receiver, &pressEvent);
        qApp->sendEvent(receiver, &releaseEvent);
        return 0;
    }

    QKeyEvent pressEvent = QKeyEvent(QEvent::KeyPress, 0, Qt::NoModifier, QString(msg));
    QKeyEvent releaseEvent = QKeyEvent(QEvent::KeyRelease, 0, Qt::NoModifier, QString(msg));
    qApp->sendEvent(receiver, &pressEvent);
    qApp->sendEvent(receiver, &releaseEvent);
    return 0;
}

public: 
QDeclarativeItem *receiver;
};

class FakekeyPlugin : public QDeclarativeExtensionPlugin
{
Q_OBJECT
  Q_PLUGIN_METADATA(IID "org.slatekit.Fakekey" FILE "fakekey.json")

public:
  void registerTypes(const char *uri)
  {
      qmlRegisterType<FakekeyModel>(uri, 1, 0, "Fakekey");
  }

};

#include "plugin.moc"
