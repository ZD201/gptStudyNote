`QButtonGroup` 是 Qt 提供的一个非常有用的类，它允许你将一组按钮（例如 `QPushButton`、`QRadioButton` 或 `QToggleButton`）组织在一起。当这些按钮中的一个被激活时，同一个组中的其他按钮将自动设置为未激活状态。这在实现单选按钮（radio buttons）或互斥按钮（toggle buttons）时非常有用。

### QButtonGroup 的用处：

1. **单选按钮组**：确保用户在同一组单选按钮中只能选择一个选项。
2. **互斥选择**：在一组按钮中，每次只有一个按钮可以被激活。
3. **简化信号处理**：你可以连接一个信号到所有的按钮，而不是每个按钮单独连接，这样可以减少代码量。
4. **管理状态**：`QButtonGroup` 提供了方便的方法来检查哪个按钮被激活，或者设置哪个按钮为激活状态。

### QButtonGroup 的用法：

1. **创建 QButtonGroup 实例**：
   ```cpp
   QButtonGroup *buttonGroup = new QButtonGroup(this);
   ```

2. **将按钮添加到组**：
   - 通过 `addButton` 方法添加按钮，并可以指定一个 ID：
     ```cpp
     buttonGroup->addButton(button1, id1);
     buttonGroup->addButton(button2, id2);
     ```
   - 使用 `QRadioButton` 的情况下，可以直接调用 `buttonGroup->addButton(radioButton);`，因为 `QRadioButton` 本身就是 `QButtonGroup` 的一个子类。

3. **连接信号和槽**：
   - 可以连接 `buttonClicked` 信号到一个槽函数，当任一按钮被点击时，该槽函数会被调用：
     ```cpp
     connect(buttonGroup, &QButtonGroup::buttonClicked, this, &MainWindow::onButtonClicked);
     ```
   - 也可以连接 `buttonPressed` 和 `buttonReleased` 信号。

4. **获取和设置当前激活的按钮**：
   - 使用 `checkedButton()` 方法获取当前被激活的按钮：
     ```cpp
     QAbstractButton *activeButton = buttonGroup->checkedButton();
     ```
   - 使用 `setId` 方法设置按钮的 ID：
     ```cpp
     buttonGroup->setId(button1, id1);
     ```
   - 使用 `button` 方法通过 ID 获取按钮：
     ```cpp
     QAbstractButton *button = buttonGroup->button(id1);
     ```

5. **设置默认行为**：
   - 如果需要，可以在初始化时设置默认的选中按钮：
     ```cpp
     button1->setChecked(true);
     ```

6. **删除按钮**：
   - 如果需要从组中移除按钮，可以使用 `removeButton` 方法：
     ```cpp
     buttonGroup->removeButton(button1);
     ```

### 示例代码：

```cpp
// 在构造函数中
QButtonGroup *buttonGroup = new QButtonGroup(this);
QPushButton *button1 = new QPushButton("Button 1", this);
QPushButton *button2 = new QPushButton("Button 2", this);
QPushButton *button3 = new QPushButton("Button 3", this);

buttonGroup->addButton(button1, 1);
buttonGroup->addButton(button2, 2);
buttonGroup->addButton(button3, 3);

connect(buttonGroup, &QButtonGroup::buttonClicked, this, &MainWindow::onButtonClicked);

// 在某个槽函数中
void MainWindow::onButtonClicked(QAbstractButton *button) {
    int id = buttonGroup->id(button);
    qDebug() << "Button with ID" << id << "clicked";
}
```

在这个示例中，我们创建了一个按钮组，并添加了三个按钮。我们连接了 `buttonClicked` 信号到一个槽函数，当按钮被点击时，槽函数会打印出被点击按钮的 ID。