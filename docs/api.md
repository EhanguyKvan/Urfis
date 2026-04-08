# 代码文档

## 系统代码

可以在除了`input()`执行期间的其他任何时候输入，必须带有前面的`/`。

参数之间用空格隔开，带有空格的为可选参数。

### /add p id (new_id) (t)

创建指定id的人物卡的一个`Character`对象，并将其加入战场

p：`p`为作为玩家方，`e`为作为敌人方

id：人物卡的`id`（即人物卡文件的文件名），同时会变成`Character`对象的`truly_id`

new_id：`Character`对象的`id`，默认为其人物卡的`id`

t：`t`为AI接管（敌人方默认），`f`为玩家操作（玩家方默认）

### /start

以现在的环境配置开始战斗

### /exit (t)

强行结束战斗

t：`t`为保留当前状态，并将其储存至本地文件（默认）。`f`为保留当前状态，但不更新本地存档

### /load name

读取战斗配置文件（存档），不能在战斗中使用

name：存档文件名（无须.txt后缀）

### /save name

保存战斗配置文件（存档）

name：存档文件名（无须.txt后缀）

### /kill id

强行杀死指定id的角色

id：该`Character`对象的`id`

### /get python_code

强制获取系统内部信息

python_code：Python代码，内置`show_class(name)`的函数，如：`/get show_class(finall_dict['p'])`

### /show (d)  

展现战场双方

d：默认为只有名字的列表，`i`为id列表，`d`为详细列表

### /set id attribute value
  
强行设定玩家的对应属性为该数值

id：要更改`Character`的`id`

attribute：`Character`的属性名称

value：要改成的值

## 局内代码

可以在战斗中，要求操作者输入局内代码时输入，不需要加`/`。

### 物品编号  

使用/装备上对应的物品

### give id 物品编号或装备代码  

给出物品或装备

id：要接收的`Character`对象的`id`

物品编号或装备代码：询问前系统会列出可用的物品列表，从前往后数其序号，即为其物品编号（从1开始）。装备代码即其对象名称，如`main_weapon`、`armor`等。

### remove 装备代码  

卸下对应的装备，如`remove accessory`

### (技能编号)  

使用对应的技能。特殊地，输入`0`即使用主武器普攻。

###escape  

逃跑

###c  

继续下个阶段

###change  

切换主副武器

## Urfis代码

狭义上的Urfis代码即写在effect属性内的代码，用以实现物品、技能效果。

## 对象类型

### 全局对象

没有`ta`的函数、方法默认的目标，具有如下属性。

|值|含义|返回值|备注|
|----|----|----|----|
|round|当前轮次数|int| |
|turn|当前回合数|int|单轮次内|
|stage|当前所处的阶段|int|1为准备阶段，2为交互阶段，3为行动阶段，4为结束阶段|

例：对自己造成“当前轮次数”点真实咒术伤害：`s.damage(round, tr, zhoushu)`

### Character对象

用于定义玩家角色的数据类型，具有以下属性。

基础属性类：

|值|含义|值|含义|
|----|----|----|----|
|id|角色标识|strength|力量（体质）|
|truly_id|人物卡标识|agility|敏捷（精巧）|
|ai|角色是否为ai（返回bool值）|wisdom|智慧（博学）|
|p_camp|角色是否为队友阵营（返回bool值）|spirit|意志（精力）|
|name|角色姓名|charm|魅力（交涉）|
|hp_max|角色最大生命值|luck|幸运|
|ap_max|角色最大ap值|fund|资金|
|level|角色等阶| | |

局内状态类：

|值|含义|值|含义|
|----|----|----|----|
|hp|角色当前生命值|is_dead|角色是否死亡（返回bool值）|
|ap|角色当前ap值|is_escape|角色是否逃跑（返回bool值）|
|place|角色当前位次|dodge|额外闪避加成|
|p_a|角色当前物理护甲|block|额外格挡加成|
|m_a|角色当前魔法护甲|round|角色现在的轮次数|
|p_r|角色当前物理护盾|turn|角色现在的回合数（回合中也计入）|
|m_r|角色当前魔法护盾|in_turn|角色是否正在自己的回合中（返回bool值）|
|o_r|角色当前其他护盾| | |

要注意的是，`p_a`和`m_a`实际上是角色的“额外护甲值”，护甲给予的护甲值不纳入计算，在受伤时也会单独计算。

装备技能类：

|值|含义|返回值|
|----|----|----|
|main_weapon|角色的主武器|Weapon对象|
|secondary_weapon|角色的副武器|Weapon对象|
|armor|角色的护甲|Armor对象|
|accessory|角色的饰品|Accessory对象|
|items|角色的物品列表|Items_list对象|
|skills|角色的技能列表|Skills_list对象|

额外的，对于Character对象，有如下与D_r对象相关的函数可以调用：

|值|含义|可选参数|
|----|----|----|
|damage_all()|角色本次战斗受到的总伤害|`p`：伤害类型|
|damage_totle()|角色本存档内受到的总伤害|`p`：伤害类型|
|damage_turn()|角色本回合受到的总伤害|`p`：伤害类型；`n`：第n个阶段内|
|damage_round()|角色本轮次受到的总伤害|`p`：伤害类型；`m`：第m个轮次内|
|damage_all_count()|角色本次战斗受到伤害的次数|`p`：伤害类型|
|damage_totle_count()|角色本存档内受到伤害的次数|`p`：伤害类型|
|damage_turn_count()|角色本回合受到伤害的次数|`p`：伤害类型；`n`：第n个阶段内|
|damage_round_count()|角色本轮次受到伤害的次数|`p`：伤害类型；`m`：第m个轮次内|

### Weapon对象

“武器对象”，用于定义主副武器的数据类型，具有以下属性。

|值|含义|值|含义|
|----|----|----|----|
|name|武器名称|from|武器来源|
|type|武器类型|desc|武器描述|
|method|武器攻击方式|effect|武器效果（返回Lej_list对象）|
|ap|武器攻击消耗的ap|level|武器等阶|

### Armor对象

“护甲对象”，用于定义护甲的数据类型，具有以下属性。

|值|含义|值|含义|
|----|----|----|----|
|name|护甲名称|from|护甲来源|
|type|护甲类型|desc|护甲描述|
|p_a|物理护甲值|effect|护甲效果（返回Lej_list对象）|
|m_a|魔法护甲值|level|护甲等阶|

### Accessory对象

“饰品对象”，用于定义饰品的数据类型，具有以下属性。

|值|含义|值|含义|
|----|----|----|----|
|name|饰品名称|desc|饰品描述|
|type|饰品类型|effect|饰品效果（返回Lej_list对象）|
|from|饰品来源|level|饰品等阶|

### Items_list对象

“物品列表对象”，用于储存玩家拥有的所有物品。

其第一项为物品数量，其余项均为`Item`对象。

例：获取伤害来源角色的物品数量

```
itemslist.list().change(d.from.items)
print(itemslist(0))
```

等效于：

```
print(len(d.from.items).minus(1))
```

### Item对象

“物品对象”，用于定义物品的数据类型，具有以下属性。

|值|含义|值|含义|
|----|----|----|----|
|name|物品名称|desc|物品描述|
|type|物品类型|effect|物品效果（返回Lej_list对象）|
|from|物品来源|level|物品等阶|
|quantity|物品数量| | |

### Skills_list对象

“技能列表对象”，用于储存玩家拥有的所有技能。

与`Items_list`类似，第一项为技能数量，其余项均为`Skill`对象。

### Skill对象

“技能对象”，用于定义技能的数据类型，具有以下属性。

|值|含义|值|含义|
|----|----|----|----|
|name|技能名称|effect|技能效果（返回Lej_list对象）|
|type|技能类型|attr|技能属性|
|from|技能来源|ap|技能消耗的ap|
|desc|技能描述| | |

### Lej对象

在程序中，技能的效果（`effect`属性中的Urfis代码）普遍以`Lej`的数据结构存储，`Lej`即`Effect`、`Loop`和`Judge`三种对象的首字母缩写，自然也包含这三种类型。

`Effect`为最基础的对象，一切最底层的数据获取、处理，都通过解析`Effect`实现。

`Loop`负责处理循环代码，其内部往往嵌套着子`Lej`对象，具有**for**和**while**两种循环模式。

`Judge`负责处理判断代码，实际上是提前封装好的，可以返回`True`或`False`的`Effect`对象。

### Lej_list对象

“效果列表对象”，用于储存`Lej`对象的列表。

与`items_list`不同，列表中只有`Lej`对象。

### Effect对象

“效果对象”，用于定义基础技能效果的数据类型。

|值|含义|返回值类型|名称来源|默认值|
|----|----|----|----|----|
|oc|效果时机|`Effect`对象|occasion|`immediate`|
|ta|效果目标| |target|`None`|
|ac|效果行为|str|action| |
|pa|效果内参数|list或`None`|parameter|`[]`或`None`|
|ex（暂时废弃）|效果外参数| |external parameter| |
|re|效果返回值| |return|`None`|

“获得一个属性”或“执行一个函数”时，都会将其属性名或函数名封装到`ac`中。前者`ta`默认值为`None`，后者默认值为`[]`。

想要快速区分，即包含`()`的代码为函数，其`ta`默认值为`[]`（零参数）。没有`()`的代码为属性，其默认值为`None`（无参数）。

### Judge对象

“判断对象”，用于定义判断的数据类型。

|值|含义|返回值类型|默认值|
|----|----|----|----|
|judgement|判断条件|`Effect`对象| |
|success|判断成功的执行内容|`Lej`对象|`None`|
|fail|判断失败的执行内容|`Lej`对象|`None`|

### Loop对象

“循环对象”，用于定义循环的数据类型。

|值|含义|返回值类型|默认值|
|----|----|----|----|
|type|循环类型|str|for或while|
|count|循环次数|int|0|
|require|循环开始时的判定条件|`Lej`对象|`None`|
|for_variable|负责遍历的变量|list[0]|`None`|
|for_list|被遍历的列表|list|`None`|
|content|循环的执行内容|`Lej_list`对象|`None`|

### Listen对象

“监听对象”，`listen`为全局唯一的监听者。

|值|含义|返回值类型|默认值|
|----|----|----|----|
|ocses|监听列表|list|[ ]|

`ocses`由`ocs`构成，`ocs`作为一个列表，遵循如下结构：

|序号|值|含义|返回值类型|
|----|----|----|----|
|0|lej_number|lej标识|int|
|1|lej_list_number|lej_list标识|int|
|2|player_id|来源角色id|str|
|3|lej_list|lej_list|`Lej_list`对象|
|4|lej|lej|`lej`对象|
|5|skill|来源技能|`Weapon`、`Item`、`Skill`对象等|

### D_r对象

“结算列表对象”，储存所有即将结算的`Damage`、`Restore`、`Damage_ap`和`Restore_ap`对象。

### Damage对象

“伤害对象”，用于定义即将结算的伤害，具有以下属性。

|值|含义|返回值类型|
|----|----|----|
|source|伤害的来源技能|`Skill`对象|
|name|伤害的来源技能的名字|str|
|from|伤害的来源角色|`Character`对象|
|type|伤害的类型|str|
|attribute|伤害的属性|str|
|value|伤害的数值|int| |
|to|伤害的目标角色|`Character`对象|
|all|伤害的目标群体|list|
|is_immunity|伤害是否已被免疫|bool|
|id|伤害标识|int|

其中，伤害类型共有五种：`p`物理伤害，`m`·魔法伤害，`o`其他伤害，`tr`真实伤害，`n`未知伤害

伤害属性参考“快速开始”页中的`attri`属性中的可能值。

伤害数值`value`在`Damage`对象创立时会被定义为`0`，并在`b_damage`结算之后再**加上**应当扣除的数值。尤其对于百分比扣除的伤害值，会直接**用此刻的hp/hp_max值**计算伤害值，然后再触发`w_damage`与`a_damage`。因此可能会出现在`w_damage`时机生命值发生变化，却仍用原生命值计算了伤害值的情况。

当治疗量溢出时，`value`会保留最初的治疗量，但实际上直接设定`hp`为`hp_max`。

当一个伤害具有多个目标时，`to`只会返回当前正在结算的角色。而在任何时候，`all`属性都会返回该伤害的所有对象所组成的列表（哪怕目标数为1，也会返回项数为1的列表）

除了玩家直接修改以外，系统修改`is_immunity`的时机只有两个：`b_damage`与`w_damage`结算完成后。而系统检测到`is_immunity == True`后，会立即中止当前伤害的结算，即不触发后续的`w_damage`或`a_damage`。

### Restore对象

“恢复对象”，用于定义即将结算的恢复，具有以下属性。

|值|含义|返回值类型|
|----|----|----|
|source|恢复的来源技能|`Skill`对象|
|name|恢复的来源技能的名字|str|
|from|恢复的来源角色|`Character`对象|
|value|恢复的数值|int| |
|to|恢复的目标角色|`Character`对象|
|all|恢复的目标群体|list|
|is_immunity|恢复是否已被免疫|bool|
|id|恢复标识|int|

### Damage_ap对象

“AP伤害对象”，用于定义即将结算的ap伤害，具有以下属性。

|值|含义|返回值类型|
|----|----|----|
|source|伤害的来源技能|`Skill`对象|
|name|伤害的来源技能的名字|str|
|from|伤害的来源角色|`Character`对象|
|value|伤害的数值|int| |
|to|伤害的目标角色|`Character`对象|
|all|伤害的目标群体|list|
|is_immunity|伤害是否已被免疫|bool|
|id|伤害标识|int|

### Restore_ap对象

“AP恢复对象”，用于定义即将结算的ap恢复，具有以下属性。

|值|含义|返回值类型|
|----|----|----|
|source|恢复的来源技能|`Skill`对象|
|name|恢复的来源技能的名字|str|
|from|恢复的来源角色|`Character`对象|
|value|恢复的数值|int| |
|to|恢复的目标角色|`Character`对象|
|all|恢复的目标群体|list|
|is_immunity|恢复是否已被免疫|bool|
|id|恢复标识|int|

## 目标

一些函数或时机，会在其**作用域**内提供一个可用的目标，即**目标域**。在目标域内，我们可以访问所给的目标（如`w_damage`会提供`d.`目标）。

作用域的起止非常好理解：作用域会延续到函数结算完毕。通俗理解，对于有多行代码的函数，则作用域为缩进的所有代码，以上面的`w_damage`为例：

```
when(w_damage){
	d.目标域
}
```

需要注意的是，同一类型的目标，其目标域会覆盖。例如以下代码：

```
when(w_damage){
	d.目标域1
	when(w_damage){
		d.目标域2
	}
	d.目标域3
}
```

两个`w_damage`会各自提供两个不同的`d.`目标，因此目标域1和3会是最外层的`d.`，目标域2的`d.`会被更新为最新的目标。

### 角色类目标

以下方法或函数可以返回`Character`或`Character_list`对象，并提供`as`目标域。

角色类目标具有两种：**一次目标**以及**二次目标**。两者具有相同的参数规则，唯一不同的是，前者是从所有“未死亡”“未逃跑”的角色中进行选择，而后者则是在已经选择好了的情况下，进行的二次选择。

此外，一次目标类的目标，会无视其前方的`ta`。而对于二次目标类的目标，如果其前方有`ta`，会将其识别为`Character`或`Character_list`对象，并在其中进行二次选择。

如果二次目标类的方法/函数不处于`as`的目标域内（即尚未进行一次选择），则会将其转换成一次目标进行选择。

例如我们要实现如下效果：当场上任意敌方玩家行动阶段开始时，对其造成10点伤害，就可以这么写：

```
when(all_enemy.b_round(3)){
	t.damage(10, m, huo)
}
```

此时由于`all_enemy`已经选择了一名玩家，`t`会在已经选中的玩家中二次选择，即已被选中的那名角色。

但有些情况下，二次选择也可以通过“将选中的玩家储存到永久列表”中的办法实现。例如我们要实现如下效果：对场上随机三个角色造成10点伤害，然后对其中一名随机角色造成额外的20点伤害，就可以这么写：

```
d_list.list().change(random_a(3))
@d_list.damage(10, m, huo)
@d_list.t.damage(10, m, huo)
```

|方法名|含义|其对应的二次目标|
|----|----|----|
|s|自己|s|
|now|当前回合的角色|now|
|target|自选敌方目标|t|
|random|随机敌方目标|ran|
|target_partner|自选友方目标|tp|
|random_partner|随机友方目标|rp|
|target_partner_e|自选除自己外的友方目标|tp_e|
|random_partner_e|随机除自己外的友方目标|rp_e|
|target_a|自选目标|ta|
|random_a|随机目标|ra|
|target_a_e|自选除自己外的目标|ta_e|
|random_a_e|随机除自己外的目标|ra_e|
|all_enemy|场上全体敌人|ts|
|all_partner|场上全体队友|ps|
|all_partner_e|场上除自己外的全体队友|ps_e|
|all|场上全体角色|as|
|all_e|场上除自己外的全体角色|as_e|

而对于以上除了`now`、`s`的所有方法，都可以当做函数使用，以target为例：

|函数|含义|
|----|----|
|target(n)|自选n个敌方目标|
|target(> n)|自选多于n个的敌方目标|
|target(< n)|自选少于n个的敌方目标|
|target(n, m)|自选多于n，少于m个敌方目标|

其中n和m都是`int`类型，当可选的目标群体只有一个目标时，系统会自动帮玩家选择，而不会询问。

### 其他目标

|方法名|含义|返回值|
|----|----|----|
|d|所受的伤害对象|`Damage`对象|
|r|恢复的体力对象|`Restore`对象|
|dap|所受的ap伤害对象|`Damage_ap`对象|
|rap|恢复的ap对象|`Restore_ap`对象|
|losed|失去的对象|`Weapon`、`Item`对象等|

## 时机

时机代码由两个部分构成：前缀与主体，两者之间用下划线`_`连接。前缀具有三种：`b`、`w`、`a`，分别对应before、when、after，即“……前”“……中”与“……后”。

只有两个时机除外：`immediate`与`only`，详见如下：

|时机|含义|备注|
|----|----|----|
|immediate|立刻|`oc`默认值，不需要人工写入|
|only(n, oc1, oc2, ...)|只有当括号内的时机同时满足大于等于n个时，才返回“时机正确”|以此来实现`and`、`or`等操作|

以下时机均需要携带前缀：

|时机主体|含义|ta|提供目标域|
|----|----|----|----|
|damaged|该角色受伤时|`Character`对象|`d.`|
|restore|该角色恢复体力时|`Character`对象|`r.`|
|damaged_ap|该角色ap受伤时|`Character`对象|`dap.`|
|restore_ap|该角色恢复ap时|`Character`对象|`rap.`|
|deadly|该角色濒死时|`Character`对象|`d.`|
|beginning|每轮开始时| | |
|round|该角色任意阶段开始时|`Character`对象|`r.`|
|dodge|该角色闪避时|`Character`对象|`dap.`|
|block|该角色格挡时|`Character`对象|`rap.`|
|lose|该角色失去装备、物品与技能时|`Character`对象|`losed.`|
|losed|该装备、物品与技能被失去时|`Weapon`、`Item`对象等|`losed.`|

## 无目标函数

以下函数均不需要目标：

### r(n, m)

投n次m面骰子，即获得0~m之间（包含0与m）的随机数。n和m均为int，返回int。

### only(n, func1, func2, ...)

只有当括号内的函数同时满足大于等于n个时，才返回`True`，否则返回`False`。

### repeat(n)

将向上n层代码重新添加至监听列表，若为最外层代码则将整个`lej_list`添加至监听列表（n不填则默认为1）。

### when(oc){ac}

```
when(oc){
	Urfis code
}
```

将作用域内全部代码，当作一个新的`Lej_list`加入`listen.ocses`监听列表中。

### if(ac){ac}

```
if(ac){
	success:
		Urfis code
	fail:
		Urfis code
}
```

创建`Judge`对象，将括号内的`ac`设定为`Judge.judgement`，并将对应的代码封装为`Lej_list`，分别设定为`Judge.success`与`Judge.fail`

当`judgement`内的条件满足时（返回`True`），则执行`success`内的内容，否则执行`fail`内的内容。

`success`和`fail`各自最多出现一次，也可以有其中一个不出现，即：

```
if(ac){
	fail:
		Urfis code
}
```

### while(ac){ac}

```
while(ac){
	Urfis code
}
```

创建`Loop`对象，将括号内的`ac`设定为`Loop.require`，并将括号内的代码设定为`Loop.content`

每次循环开始时，判断`Loop.require`的表达式是否通过（返回`True`），若是则执行`Loop.content`内的代码。

### continue(n)

终止向上n层作用域的`Lej`结算（适用于`Loop`和`Judge`）

### break()

终止当前`Lej_list`结算

## 有目标函数

### 变量与列表

Urfis中并不支持“自定义变量”，只支持“自定义列表”。因此要实现变量的效果，可以转换为列表的第一项。

列表分为三种：普通列表、临时列表与全局列表，他们的区别在于其存活时间。普通列表的作用域为“从创建开始，到本次战斗结束”，临时列表的作用域为“从创建时开始，到`listen.check()`结束”。全局列表的作用域为“从创建时开始，本存档内永不销毁”。

临时列表以`_`开头，全局变量以`^`开头。

特别的，对于`for`函数来说，如果遍历的变量为临时变量，那么会在`for`函数作用域结束时，将其销毁。

要调用列表，可以通过以下代码：`@name`（返回整个列表，list）或`@name(n)`（返回列表第n项）。返回的变量永远为**非拷贝**类型，即`@name`返回的是名为name的列表的地址，而非其内容。

另外对于`for`函数而言，其括号内的是其变量。程序在处理`for`函数时会自动将`@i`赋值为`@i(1)`。因此在`Loop.content`中，无需再使用`@i(1)`，而是可以直接使用`@i`。

以下为列表的方法与函数，其`ta`无一例外均需要为str类型：

|函数|含义|备注|
|----|----|----|
|list(n, m, k, ...)|创建名为`ta`，值为[n, m, k]的列表。或修改`ta`列表的值为[n, m, k]|返回该列表，参数也可以留空以创建空列表|
|plus_list()|将名为`ta`的列表中每一个纯数字项相加|返回int|
|append(n)|让n成为名为`ta`的列表的新元素，若没有该列表则新建列表|可以有任意个参数|
|sample(n)|随机取出名为`ta`的列表的n项|返回list|
|assign_o(n, m)|让名为`ta`的列表的第n项重新赋值为m|项数从0开始|
|assign_n(n, m)|让名为`ta`的列表第一个值为n的项，重新赋值为m|项数从0开始|
|assign_a(n, m)|让名为`ta`的列表所有值为n的项，重新赋值为m|项数从0开始|

### 计算

以下函数支持str、int、和list类型数据的加减，int类型数据的乘除（若不是int则乘除函数会返回0）。

|函数|返回值|
|----|----|
|plus()|`ta` + `pa`|
|minus()|`ta` - `pa`|
|multiply()|`ta` * `pa`|
|divide()|`ta` / `pa`|

### for(@_i){ac}

``` 
ta.for(@_i){
	Urfis code
}
```

创建`Loop`对象，将括号内的变量设定为`Loop.for_variable`，将`ta`（list）设定为`Loop.for_list`，并将括号内的代码设定为`Loop.content`

循环`ta`的项数次，每次让`for_variable`赋值为列表的第“当前循环数”项

### insert()

悬置当前结算，并强行结算`ta`。`ta`可以为`Lej`或`Lej_list`，若为前者，则会自动转换为`Lej_list`，并返回其返回值。

### damage(n, m, huo, np)

对`ta`（`Character`对象）造成伤害

n：伤害数值，int类型。

m：伤害类型，包括`m`魔法、`p`物理、`tr`真实、`o`其他、`n`未知。

huo：伤害属性，参考“快速开始”页中的`attri`属性中的可能值。

np：可选参数，若不写则“造成n点伤害”。若为“np”则为“造成n%的当前hp点伤害”，若为“op”则为“造成n%的最大hp点伤害”。

### restore(n, np)

令`ta`（`Character`对象）恢复体力

n：伤害数值，int类型。

np：可选参数，若不写则“恢复n点血量”。若为“np”则为“恢复n%的当前hp点血量”，若为“op”则为“恢复n%的最大hp点血量”。

### damage_ap(n)

令`ta`（`Character`对象）失去ap

n：伤害数值，int类型。

### restore_ap(n)

令`ta`（`Character`对象）恢复ap

n：伤害数值，int类型。

### plus_b(strength, n)

令`ta`（`Character`对象）本次战斗增加n点属性

strength：属性类型，str类型。除了七项基本属性外，还可以填写`dodge`和`block`，则分别增加其“闪避附加值”和“格挡附加值”。

n：增加值，int类型。

### plus_g(strength, n)

令`ta`（`Character`对象）本存档中增加n点属性

strength：属性类型，str类型。除了七项基本属性外，还可以填写`dodge`和`block`，则分别增加其“闪避附加值”和“格挡附加值”。

n：增加值，int类型。

### set_flag(name)

在`ta`（`Character`对象）身上放置一个名为`name`（str类型）的不可见标记。

### remove_flag(name, n)

去除`ta`（`Character`对象）身上n个名为`name`（str类型）的不可见标记。

若没有指定`ta`，则去除全场所有角色的`name`标记。

若没有`n`，则去除所有`name`标记。

### devastate()

销毁目标类。

`ta`可以为`Weapon`、`Armor`、`Accessory`、`Item`、`Skill`对象。

若没有指定`ta`，则默认销毁本技能。玩家失去本技能，终止当前结算，并从`ocses`中销毁一切源自本技能的`ocs`。

### change(n)

将`ta`强行修改为n。

### owner()

获得`ta`的拥有者，返回`Character`对象。

`ta`可以为`Weapon`、`Armor`、`Accessory`、`Item`、`Skill`对象。

### input()

给玩家一个输入的机会，并返回输入的内容。

### force(){}

```
Skill.force() {
	s.define(t)
	t.define(t)
}
```

暂时废弃。

强制插入结算`ta`（`Skill`对象），并提供`define`定义的目标域。

## 判断类函数

以下函数均只有两种返回值：`True`和`False`。

### have_flag(name)

检测`ta`（`Character`对象）是否拥有名为name的不可见标记。

若没有指定`ta`，则检测全场玩家的`name`标记。

若没有`name`，则检测所有标记。

### in(n)

检测n中是否有`ta`。

### vs(n, m)

令n和m拼点，n和m均为任意int类型的对象，拼点结果为基础值+r2d8。

若n拼点结果大于m的拼点结果，则返回`True`，反之返回`False`。

例：令自己的敏捷和伤害来源的力量进行拼点：

```
if(vs(s.agility, d.from.strength)){
	success:
		Urfis code
}
```

### more(n, m)

检测n是否大于m。

### less(n, m)

检测n是否小于m。

### equal(n, m)

检测n是否等于m。

### not(n)

检测n表达式是否不成立。