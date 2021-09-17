# Cucumber教程

## 一、安装环境搭建——Java

pom.xml文件导入一下依赖包：

~~~xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-java8</artifactId>
    <version>6.10.4</version>
    <scope>test</scope>
</dependency>

<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-java</artifactId>
    <version>6.10.4</version>
    <scope>test</scope>
</dependency>

<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-junit</artifactId>
    <version>6.10.4</version>
    <scope>test</scope>
</dependency>
~~~

## 二、Gherkin语法

Gherkin 使用一组特殊的**关键字**来赋予可执行规范的结构和意义。每个关键字都翻译成许多口语：在此参考中，我们将使用英语。

格尔金文档中的大多数行都以**其中一个关键字**开头。

评论仅在新行的开头（功能文件中的任意一处）允许。它们以零或更多空间开始，然后是哈希标志 （`#`） 和一些文本。

块评论目前不支持盖尔金。

空间或选项卡可用于凹痕。建议的凹痕级别为两个空格。下面是一个示例：

~~~xml
Feature: Guess the word

  # The first example has two steps
  Scenario: Maker starts a game
    When the Maker starts a game
    Then the Maker waits for a Breaker to join

  # The second example has three steps
  Scenario: Breaker joins a game
    Given the Maker has started a game with the word "silky"
    When the Breaker joins the Maker's game
    Then the Breaker must guess a word with 5 characters
~~~

每个步骤的尾随部分（在关键字之后）与代码块（称为**步骤定义**）匹配。

### 关键字

每行不是空白行必须从 Gherkin*关键字*开始，然后是任何您喜欢的文本。唯一的例外是功能和场景描述。

主要关键词是：

- Feature
- Rule (as of Gherkin 6)
- Example (or `Scenario`)
- Given, When, Then, And, But for steps (or *）
- Background
- Scenario Outline
- Examples (or Scenarios)

还有几个次要关键字：

- `"""` (文字符串)
- `|` (数据表)
- `@` (标签)
- `#` (评论)

#### 本地化

格尔金是本地化的许多口语：每个都有自己的本地化等价物这些关键字。

#### Feature

关键字Feature的目的是提供软件功能的高级别描述，以及组相关方案。

Gherkin 文档中的第一个主要关键字必须始终是Feature，然后`:`后面描述该功能的简短文本。

您可以在`Feature`下面添加自由形式文本以添加更多描述。

这些描述行在运行时被黄瓜忽略，但可用于报告（它们包含在报告工具（如官方 HTML 格式）中）。

~~~xml
Feature: Guess the word

  The word guess game is a turn-based game for two players.
  The Maker makes a word for the Breaker to guess. The game
  is over when the Breaker guesses the Maker's word.

  Example: Maker starts a game
~~~

名称和可选描述对Cucumber没有特殊意义。他们的目的是为您提供一个记录功能重要方面的地方，例如简短的解释和业务规则列表（一般接受标准）。

当您开始与关键字Background,Rule,Example或Scenario Outline（或他们的别名关键字）的行开始行时，结束Feature的免费格式描述。

您可以将标签放在Feature上面，以组相关功能，独立于您的文件和目录结构。

#### 描述

自由形式的描述(如上面描述的特征)也可以放在 `Example`/`Scenario`, `Background`, `Scenario Outline` and `Rule`下面。

你可以写任何你喜欢的东西，只要没有一行以关键字开头。

描述可以是Markdown格式，包括官方HTML格式支持这一点。

#### 统治

(可选的)Rule关键字从v6开始就是Gherkin的一部分。

#### Cucumber支持规则

Rule关键字仍然是相当新的。它已经被移植到许多Cucumber实现中。但是，如果遇到问题，请检查Cucumber实现的文档，以确保它支持它。

Rule关键字的目的是表示一个应该实现的业务规则。它提供了特性的附加信息。规则用于将属于此业务规则的几个场景分组在一起。规则应该包含一个或多个演示特定规则的场景。

例如：

~~~xml
# -- FILE: features/gherkin.rule_example.feature
Feature: Highlander

  Rule: There can be only One

    Example: Only One -- More than one alive
      Given there are 3 ninjas
      And there are more than one ninja alive
      When 2 ninjas meet, they will fight
      Then one ninja dies (but not me)
      And there is one ninja less alive

    Example: Only One -- One alive
      Given there is only 1 ninja alive
      Then he (or she) will live forever ;-)

  Rule: There can be Two (in some cases)

    Example: Two -- Dead and Reborn as Phoenix
      ...
~~~

#### 例

这是一个说明业务规则的具体示例。它由一系列步骤组成。

关键字“Scenario”是关键字“Example”的同义词。

您可以有任意多的步骤，但我们建议每个示例3-5个步骤。过多的步骤将导致示例失去其作为规范和文档的表达能力。

除了作为一个规范和文档，一个例子也是一个测试。总的来说，您的示例是系统的可执行规范。

例子遵循同样的模式：

- 描述初始上下文(Given 步骤)
- 描述一个事件(When步骤)
- 描述一个预期的结果(Then步骤)

#### Steps

每一步都以Given, When, Then, And or But开始。

Cucumber按照您所编写的顺序执行场景中的每一步。当Cucumber尝试执行一个步骤时，它会寻找要执行的匹配步骤定义。

在查找步骤定义时，不考虑关键字。这意味着你不能有一个`Given,When,Then,And`或`But`步骤与另一个步骤相同的文本。

黄瓜考虑以下步骤重复：

~~~xml
Given there is money in my account
Then there is money in my account
~~~

这似乎是一个限制，但它迫使你想出一个不那么模棱两可，更明确的域名语言

~~~xml
Given my account has a balance of £430
Then my account should have a balance of £430
~~~

#### Given

`Given`步骤用于描述系统的初始上下文 - 场景*场景*。这通常是*过去*发生的事情。

当Cucumber执行一个`Given`步骤时，它会将系统配置为处于定义良好的状态，例如创建和配置对象或将数据添加到测试数据库。

给定步骤的目的是在用户(或外部系统)开始与系统交互之前(在When步骤中)将系统置于已知状态。避免在给定的中讨论用户交互。如果您正在创建用例，Given将是您的先决条件。

有几个Given步骤是可以的(对于第2或以上的步骤，使用And或But使其更具可读性)。

例子：

~~~xml
Mickey and Minnie have started a game #米奇和米妮已经开始玩游戏了
I am logged in #我登录了
Joe has a balance of £42 #乔的余额为 42 英镑
~~~

#### When

`When`步骤用于描述事件或*行动*。这可能是一个人与系统交互，也可以是另一个系统触发的事件。

强烈建议每个场景只使用一个When步骤。如果你觉得有必要添加更多内容，这通常是一个信号，表明你应该将场景分成多个场景。

例子：

~~~xml
Guess a word #猜猜一个字
Invite a friend #邀请朋友
Withdraw money #取款
~~~

**想象一下是1922年**

> 大多数软件都做人们可以手动完成的事情（只是效率不高）。
>
> 努力想出一些对技术或用户界面没有任何假设的例子。想象一下，那是1922年，那时没有电脑。
>
> 实施细节应隐藏在**步骤定义中**。

#### Then

`Then`步骤用于描述*预期*结果或结果。

**步骤`Then`的步骤定义**应使用*断言*将*实际*结果（系统实际执行的）与*预期*结果（步骤表示系统应该执行的）进行比较。

结果*应该是*可**观察到的**产出。即，系统*中出现*的（报告、用户界面、消息），而不是系统中深埋的行为（如数据库中的记录）。

例子：

- 看到猜对的字错了
- 收到邀请
- 卡应该吞下

虽然实施在数据库中查看的步骤可能很诱人 - 抵制这种诱惑！`Then`

您只能验证用户（或外部系统）可以观察到的结果，而对数据库的更改通常不会。

#### And，But

如果你有连续的，`Given`的， `When`的或 `Then`的，*你可以*写：

```gherkin
Example: Multiple Givens
  Given one thing
  Given another thing
  Given yet another thing
  When I open my eyes
  Then I should see something
  Then I shouldn't see something else
```

或者，你可以用And和But代替Given、When和Then，使这个例子结构更加流畅:

```gherkin
Example: Multiple Givens
  Given one thing
  And another thing
  And yet another thing
  When I open my eyes
  Then I should see something
  But I shouldn't see something else
```

#### *

Gherkin 还支持使用星号 （`*`） 来代替任何正常的步骤关键字。当您有一些步骤实际上是*一个列表的东西*，所以你可以表达它更像要点，否则等的自然语言`And`可能不会读得那么优雅。

例如：

```gherkin
Scenario: All done
  Given I am out shopping
  And I have eggs
  And I have milk
  And I have butter
  When I check my list
  Then I don't need anything
```

可表示为：

```gherkin
Scenario: All done
  Given I am out shopping
  * I have eggs
  * I have milk
  * I have butter
  When I check my list
  Then I don't need anything
```

#### 背景

偶尔你会发现自己重复相同的`Given`步骤，在所有的场景在一个`Feature`。 

由于在每种情况下都重复使用，这表明这些步骤对于描述这些方案并非*必不可少*：它们是*偶然的细节*。您可以通过将这些步骤分组到一个`Background`部分，将这些`Given`步骤从字面上移动到后台。

一个`Background`允许您在后续场景中添加一些上下文。它可以包含一个或多个`Given`步骤，这是*运行之前，每个*场景，但在任何前钩之后。 

一个`Background` 放在第一个`Scenario` /`Example`， 在同一水平的凹痕。

例如：

```gherkin
Feature: Multiple site support
  Only blog owners can post to a blog, except administrators,
  who can post to all blogs.

  Background:
    Given a global administrator named "Greg"
    And a blog named "Greg's anti-tax rants"
    And a customer named "Dr. Bill"
    And a blog named "Expensive Therapy" owned by "Dr. Bill"

  Scenario: Dr. Bill posts to his own blog
    Given I am logged in as Dr. Bill
    When I try to post to "Expensive Therapy"
    Then I should see "Your article was published."

  Scenario: Dr. Bill tries to post to somebody else's blog, and fails
    Given I am logged in as Dr. Bill
    When I try to post to "Greg's anti-tax rants"
    Then I should see "Hey! That's not your blog!"

  Scenario: Greg posts to a client's blog
    Given I am logged in as Greg
    When I try to post to "Expensive Therapy"
    Then I should see "Your article was published."
```

Background也支持该Rule级别，例如：

~~~xml
Feature: Overdue tasks
  Let users know when tasks are overdue, even when using other
  features of the app

  Rule: Users are notified about overdue tasks on first use of the day
    Background:
      Given I have overdue tasks

    Example: First use of the day
      Given I last used the app yesterday
      When I use the app
      Then I am notified about overdue tasks

    Example: Already used today
      Given I last used the app earlier today
      When I use the app
      Then I am not notified about overdue tasks
  ...
~~~

每个`Feature`或`Rule`只能有一组Background步骤。如果不同场景需要不同的`Background`步骤，可以考虑将场景集分解为更多`Rule`或更多`Feature`。

对于Background的较不明确的替代，请查看条件钩子。

#### 使用背景的提示

- 不要用`Background`来设置**复杂的状态**，除非该状态实际上是客户需要知道的。
  - 例如，如果用户和站点名称对客户端并不重要，请使用更高级别的步骤，例如 。`Given I am logged in as a site owner`
- 保持你的`Background`部分**短**。
  - 客户在阅读场景时需要真正记住这些内容。如果`Background`超过 4 行长，请考虑将一些不相关的细节移动到更高级别的步骤中。
- 使你的Background部分**生动**。
  - 使用五颜六色的名字，并尝试讲述一个故事。人脑对故事的跟踪比跟踪名字（如`"User A" "User B" "Site 1"`等等）要好得多。
- 保持你的场景**短**，不要有太多。
  - 如果该`Background`部分已从屏幕上滚动下来，则读取器将不再对正在发生的事情进行完整概述。考虑使用更高级别的步骤，或拆分`*.feature`文件。

#### 方案大纲

关键字Scenario Outline可用于多次运行相同的Scenario，具有不同的值组合。

关键词Scenario Template是关键词的同义词Scenario Outline。

复制和粘贴场景以快速使用不同的值变得乏味和重复：

~~~xml
Scenario: eat 5 out of 12
  Given there are 12 cucumbers
  When I eat 5 cucumbers
  Then I should have 7 cucumbers

Scenario: eat 5 out of 20
  Given there are 20 cucumbers
  When I eat 5 cucumbers
  Then I should have 15 cucumbers
~~~

我们可以将这两个类似的场景分解为Scenario Outline（场景大纲）。

通过使用带有< >分隔参数的模板，场景概要允许我们更简明地表达这些场景：

~~~xml
Scenario Outline: eating
  Given there are <start> cucumbers
  When I eat <eat> cucumbers
  Then I should have <left> cucumbers

  Examples:
    | start | eat | left |
    |    12 |   5 |    7 |
    |    20 |   5 |   15 |
~~~

#### 例子

一个Scenario Outline必须包含一个Examples （或Scenarios） 部分。其步骤被解释为从未直接运行的模板。相反，在其下方部分的Examples*每个行Scenario Outline运行一次*（不包括第一个头行）。

这些步骤可以使用引用示例表中的头的<>分隔参数。Cucumber在尝试将步骤与步骤定义匹配*之前*，会用表中的值替换这些参数。

您也可以在**多行步进参数**中使用参数。

### 步骤参数

在某些情况下，您可能希望将更多的数据传递到某个步骤，而不是单行中拟合。为此目的，Gherkin有Doc Strings和Data Tables.

#### 文档字符串

`Doc Strings`将较大的文本传递到步骤定义是很方便的。

案文应由划界者抵消，这些划界线由三个双报价标记组成：

~~~xml
Given a blog post named "Random" with Markdown body
  """
  Some Title, Eh?
  ===============
  Here is the first paragraph of my blog post. Lorem ipsum dolor sit amet,
  consectetur adipiscing elit.
  """
~~~

在您的步骤定义中，无需查找此文本并将其与您的模式匹配。它将自动通过，作为步骤定义中的最后一个参数。

凹痕的开口并不重要，虽然常见做法是两个空间从封闭步骤。然而，三重报价内的凹痕意义重大。Doc 字符串的每个行将根据打开口进行分调。因此，将保留开口柱外的凹痕。`""" """ """`

Doc 字符串还支持使用三个背记作为划界器：

~~~xml
Given a blog post named "Random" with Markdown body
  ```
  Some Title, Eh?
  ===============
  Here is the first paragraph of my blog post. Lorem ipsum dolor sit amet,
  consectetur adipiscing elit.
  ```
~~~

对于那些习惯于用马克当写作的人来说，这可能是很熟悉的。

**背板的工具支持**

虽然所有当前版本的黄瓜支持背记作为划界器，但许多工具（如文本编辑器）还没有。

#### 数据表

`Data Tables`将值列表传递到步骤定义是很方便的：

~~~xml
Given the following users exist:
  | name   | email              | twitter         |
  | Aslak  | aslak@cucumber.io  | @aslak_hellesoy |
  | Julien | julien@cucumber.io | @jbpros         |
  | Matt   | matt@cucumber.io   | @mattwynne      |
~~~

就像，将传递到步骤定义作为最后一个参数。`Doc Strings Data Tables`

#### 桌细胞逃逸

如果你想在桌上使用新字符，你可以写成。如果你需要一个作为细胞的一部分，你可以逃脱它作为.最后，如果你需要一个，你可以逃脱它与。`\n | \| \ \\`

#### 数据表 API

黄瓜提供了丰富的 API，用于从步骤定义中操纵表。有关详细信息，请参阅**数据表 API 参考**。

### 口语

您为 Gherkin 选择的语言应该是您的用户和域专家在讨论域名时使用的语言相同。应避免在两种语言之间翻译。

这就是为什么格尔金已被翻译成**70多种语言**。

以下是用挪威语编写的盖尔金场景：

~~~xml
# language: no
Funksjonalitet: Gjett et ord

  Eksempel: Ordmaker starter et spill
    Når Ordmaker starter et spill
    Så må Ordmaker vente på at Gjetter blir med

  Eksempel: Gjetter blir med
    Gitt at Ordmaker har startet et spill med ordet "bløtt"
    Når Gjetter blir med på Ordmakers spill
    Så må Gjetter gjette et ord på 5 bokstaver
~~~

功能文件第一行的头条告诉黄瓜要使用什么口语 - 例如法语。如果您省略此头，黄瓜将默认为英语 （）。`# language: # language: fr en`

一些黄瓜实现还允许您在配置中设置默认语言，因此您不需要将头放入每个文件中。`# language`

