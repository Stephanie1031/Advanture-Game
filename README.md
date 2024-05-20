# Adventure Game!
视频学习地址：[OOP learning](https://www.bilibili.com/video/BV1qm4y1L7y1/?spm_id_from=333.337.search-card.all.click&vd_source=a8574cdad20c76898bbf31ecbbdc2516)

我们将会实现一个基于文本的冒险游戏。你可以通过执行`adventure.py`来开始游戏。

通过命令`Ctrl-C`或者`Ctrl-D`退出游戏。

为了使得实验结果更加直观，特意提供完成后的EXE版本供参考~详情可点击`adventure`文件夹中的可执行文件进行游玩。
## Compulsory Questions
### Q1: Who am I?
首先，你需要为你自己在`data.py`中创建一个`Player`对象。看一下`classes.py`中`Player`类的定义，在`data.py`底部创建一个`Player`对象。
`Player`构造函数接收两个参数：

- `name`是你喜欢的名字（string类型）
- 开始的位置`place`

你的玩家将会从`sather_gate`开始，改动代码位置：
```python
    Player:
    The Player should start at sather_gate.
    "*** YOUR CODE HERE ***"
    me = None
```
当你创建完成之后，就可以启动游戏了`adventure.py`

你会看到下面这些输出（可能顺序有所不同）：
```
    Welcome to the adventure game!

    It's a bright sunny day.
    You are a bright and eager CS class student named xxx, wandering around the campus looking for some snacks before the study party.

    Let's go to FSM (Free Speech Movement Cafe) and see what we can find there!

    There are 7 possible commands:
        look
        go to [place]
        take [thing]
        talk to [character]
        check backpack
        help
        unlock [place]
```
最后玩家需到达`HP Auditorium`参加学习聚会，但在此之前，我们可以指挥玩家在校内走动并学习聚会准备一些零食与桌游😊

目前你现在除了look什么都做不了，让我们来继续完善它！
### Q2: Where do I go?
首先，我们需要能够移动到不同的地方。如果你试着使用`go to`命令，你会发现什么也没有发生。

在`classes.py`中，完成`Player`类的`go_to`方法，让地点没有锁住的情况下，它能够将你的`place`属性更新成`destination_place`，并且你还需要输出当前位置的名称。

代码框架：
```python
    def go_to(self, location):
        """Go to a location if it's among the exits of player's current place.

        >>> sather_gate = Place('Sather Gate', 'Sather Gate', [], [])
        >>> gbc = Place('GBC', 'Golden Bear Cafe', [], [])
        >>> sather_gate.add_exits([gbc])
        >>> sather_gate.locked = True
        >>> gbc.add_exits([sather_gate])
        >>> me = Player('player', sather_gate)
        >>> me.go_to('GBC')
        You are at GBC, Golden Bear Cafe - Now with (healthy?) food.
        >>> me.place is gbc
        True
        >>> me.place.name
        'GBC'
        >>> me.go_to('GBC')
        Can't go to GBC from GBC.
        Try looking around to see where to go.
        You are at GBC, Golden Bear Cafe - Now with (healthy?) food.
        >>> me.go_to('Sather Gate')
        Sather Gate is locked! Go look for a key to unlock it
        You are at GBC, Golden Bear Cafe - Now with (healthy?) food.
        """
        destination_place = self.place.get_neighbor(location)
        if destination_place.locked:
            print(destination_place.name, 'is locked! Go look for a key to unlock it')
        "*** YOUR CODE HERE ***"
```
使用`ok`命令来进行测试，可以在terminal中输入`python3 ok -q Player.go_to`

当你完成了这个问题之后，你将可以移动到不同的位置并且进行查看（look）。为了完善游戏的功能，包括和NPC交谈以及捡起东西，你需要完成以下可选问题。
## Optional Questions
### Q3: How do I talk?
现在你已经可以去你想去的地方了，试着去往`Wheeler`。在那里你可以找到`Jerry`。通过`talk to`命令和它对话。这现在仍然不会生效。

接着，实现`Player`中的`talk_to`方法。`talk_to`接收一个角色（Character）的名称，并且打印出它的反应。查看下面代码获取更多细节。

提示：`talk_to`接收一个参数`person`，它是一个字符串。`self.place`中的实例属性`characters`是一个字典，将角色的名称和角色的对象映射起来。
当你拿到角色对象之后，你需要用`Character`类中的什么方法来进行交谈呢？
```python
    def talk_to(self, person):
        """Talk to person if person is at player's current place.

        >>> jerry = Character('Jerry', 'I am not the Jerry you are looking for.')
        >>> wheeler = Place('Wheeler', 'You are at Wheeler', [jerry], [])
        >>> me = Player('player', wheeler)
        >>> me.talk_to(jerry)
        Person has to be a string.
        >>> me.talk_to('Jerry')
        Jerry says: I am not the Jerry you are looking for.
        >>> me.talk_to('Tiffany')
        Tiffany is not here.
        """
        if type(person) != str:
            print('Person has to be a string.')

        "*** YOUR CODE HERE ***"
```
`Character`类的定义：
```python
class Character(object):
    def __init__(self, name, message):
        self.name = name
        self.message = message

    def talk(self):
        return self.message
```
### Q4: How do I take items?
现在让我们实现`take`命令，让玩家可以往`backpack`中放入道具。目前，你没有背包（`backpack`），所以让我们创建一个实例变量`backpack`，将它初始化成空的`list`。

当你初始化你的空背包之后，实现`take`方法，它接收一个物品的名称，检查你所在的位置是否有这件道具（`Thing`），接着将它放入你的背包。查看代码，获取更多细节。

提示：`things`是`Place`类的实例属性，它将物品名称和对象映射起来。
`Place`类中的`take`方法也能派上用场，它的作用是使得物品被拿走后该地的物品被移除。
```python
    def take(self, thing):
        """Take a thing if thing is at player's current place

        >>> lemon = Thing('Lemon', 'A lemon-looking lemon')
        >>> gbc = Place('GBC', 'You are at Golden Bear Cafe', [], [lemon])
        >>> me = Player('Player', gbc)
        >>> me.backpack
        []
        >>> me.take(lemon)
        Thing should be a string.
        >>> me.take('orange')
        orange is not here.
        >>> me.take('Lemon')
        Player takes the Lemon
        >>> me.take('Lemon')
        Lemon is not here.
        >>> isinstance(me.backpack[0], Thing)
        True
        >>> len(me.backpack)
        1
        """
        if type(thing) != str:
            print('Thing should be a string.')
            
        "*** YOUR CODE HERE ***"
```
使用ok命令来测试：`python3 ok -q Player.take`
### Q5: No door can hold us back!
`FSM`锁上了，我们没有办法进去。而你已经对无法喝到甜美可口的咖啡感到非常绝望了。

为了进入FSM并且摄入咖啡因，我们需要做两件事情。首先，我们需要创建一个新的类型`Key`，它是`Thing`的子类，但重载了`use`方法来打开`FSM`的门。

提示1：`Place`有一个`locked`实例属性，你可能需要改动它

提示2：我们拿到的是要开启的地点的string，而不知道这个名称对应的对象。这需要我们使用`Place`的`get_neighbour`函数。
```python
def unlock(self, place):
    """If player has a key, unlock a locked neighboring place.

    >>> key = Key('SkeletonKey', 'A Key to unlock all doors.')
    >>> gbc = Place('GBC', 'You are at Golden Bear Cafe', [], [key])
    >>> fsm = Place('FSM', 'Home of the nectar of the gods', [], [])
    >>> gbc.add_exits([fsm])
    >>> fsm.locked = True
    >>> me = Player('Player', gbc)
    >>> me.unlock(fsm)
    Place must be a string
    >>> me.go_to('FSM')
    FSM is locked! Go look for a key to unlock it
    You are at GBC
    >>> me.unlock(fsm)
    Place must be a string
    >>> me.unlock('FSM')
    FSM can't be unlocked without a key!
    >>> me.take('SkeletonKey')
    Player takes the SkeletonKey
    >>> me.unlock('FSM')
    FSM is now unlocked!
    >>> me.unlock('FSM')
    FSM is already unlocked!
    >>> me.go_to('FSM')
    You are at FSM
    """
    if type(place) != str:
        print("Place must be a string")
        return
    key = None
    for item in self.backpack:
        if type(item) == Key:
            key = item
    "*** YOUR CODE HERE ***"
```
```python
class Thing(object):
    def __init__(self, name, description):
        self.name = name
        self.description = description

    def use(self, place):
        print("You can't use a {0} here".format(self.name))

""" Implement Key here! """
```
### Q6: 最简单的一集
现在你可以在校园里到处走动以及尝试着赢得游戏了。和各个地方的人交谈来获取提示。你能拯救这一天并且赶上的课程聚会吗？

Have a great time!
