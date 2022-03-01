# musikBearLMMS
Qt button
Here is an example:

#ifndef DUALACTIONPUSHBUTTON_H
#define DUALACTIONPUSHBUTTON_H

#include <QPushButton>


class DualActionPushButton: public QPushButton
{
    Q_OBJECT
public:
    explicit DualActionPushButton(QWidget *p = nullptr);
    ~DualActionPushButton();
protected:
    void mousePressEvent(QMouseEvent *event);

};

#endif // DUALACTIONPUSHBUTTON_H

#include <QMouseEvent>
#include <QDebug>

DualActionPushButton::DualActionPushButton(QWidget *p):
    QPushButton(p)
{ }

DualActionPushButton::~DualActionPushButton()
{ }

void DualActionPushButton::mousePressEvent(QMouseEvent *event)
{
    if (event->button() == Qt::LeftButton) {
        qDebug() << "Left";
        // trigger the first action in the button's action list (if there is one).
        QList<QAction*> acts = this->actions();
        if (acts.count() > 0) {
            acts.at(0)->trigger();
            event->accept();
        }
    }

    else if (event->button() == Qt::RightButton) {
        qDebug() << "Right";
        // trigger the second action in the button's action list (if there is one)
        QList<QAction*> acts = this->actions();
        if (acts.count() > 1) {
            acts.at(1)->trigger();
            event->accept();
        }
    }
    // Ignore other options
    event->ignore();
}

and the setup

    QAction* strumNotesUpAction =
    new QAction(tr( "Strum Selected by Q-value" ), this );
    connect( strumNotesUpAction, &QAction::triggered,
             [=]() { qDebug() << "strumUp triggered"; } );

    //2. button
    QAction* strumNotesDnAction =
    new QAction(tr( "Strum Selected by Q-value" ), this );
    connect( strumNotesDnAction, &QAction::triggered,
             [=]() { qDebug() << "strumDwn triggered"; } );

    QPushButton *pushButton m= new DualActionPushButton(this);
    pushButton->addAction(strumNotesUpAction);
    pushButton->addAction(strumNotesDnAction);
    // insert into your tool bar
