🐾 IntelliJ IDEA

🕘 2019.11.22 由 hoanfirst 编辑

## 常用快捷键

### 十大快捷键

1. 智能提示、快速修复、自动补全

用**F2/ Shift+F2**移动到有错误的代码，**Alt+Enter**快速修复(即Eclipse中的Quick Fix功能)。

当智能提示为我们自动补全方法名时，我们通常要自己补上行尾的反括号和分号，当括号嵌套很多层时会很麻烦，这时我们只需敲**Ctrl+Shift+Enter**就能自动补全末尾的字符。而且不只是括号，例如敲完if/for时也可以自动补上{}花括号。

Intellij能够智能感知Spring、Hibernate等主流框架的配置文件和类，以静制动，在看似“静态”的外表下，智能地扫描理解你的项目是如何构造和配置的。



2. 重构

无敌的重构功能大汇总快捷键**Ctrl+Shift+Alt+T**，叫做Refactor This

**Ctrl+Alt+V** 提取变量时自动检查到所有匹配同时提取成一个变量

**shift F6** （refractor | rename）直接改名



3. 代码生成

有**fori/sout/psvm+Tab**即可生成循环、System.out、main方法等样板代码（用 **ctrl J** 查看所有模板）

**Alt+Insert**，在编辑窗口中点击可以生成构造函数、toString、getter/setter、重写父类方法等。

后缀自动补全功能(**Postfix Completion**)，比模板生成更加灵活和强大。例如要输入for(User user : users)只需输入user**.for+Tab**。

Date birthday = user.getBirthday();只需输入user.getBirthday()**.var+Tab**



4. 编辑

能够自动按语法选中代码的**Ctrl+W**以及反向的**Ctrl+Shift+W**

**Ctrl+Left/Right**移动光标到前/后单词，**Ctrl+[/]**移动到前/后代码块

**alt top/dowm** 移动光标该文件内的不同方法

**Ctrl+Y** 和 **Ctrl+X** 删除行

**Ctrl+C** 复制行

**Ctrl+D**复制行到下一行

**Ctrl+</>**折叠代码

Intellij的书签功能也是不错的，用**Ctrl+Shift+Num**定义1-10书签(再次按这组快捷键则是删除书签)，然后通过**Ctrl+Num**跳转。这避免了多次使用前/下一编辑位置**Ctrl+Left/Right**来回跳转的麻烦



5. 查找打开

earch Everywhere功能，只需按**Shift+Shift**即可在一个弹出框中搜索任何东西，包括类、资源、配置项、方法等等。

类的继承关系则可用**Ctrl+H**打开类层次窗口，在继承层次上跳转则用**Ctrl+B/Ctrl+Alt+B**分别对应父类或父方法定义和子类或子方法实现，查看当前类的所有方法用**Ctrl+F12**。

要查找某个类或方法的使用，**Alt+F7**。

要查找文本的出现位置就用**Ctrl+F/Ctrl+Shift+F**在当前窗口或工程中查找，再配合**F3（Enter）/Shift+F3**前后移动到下一匹配处。



5. 其他辅助

Ø  **命令**：**Ctrl+Shift+A**可以查找所有Intellij的命令，并且每个命令后面还有其快捷键。所以它不仅是一大神键，也是查找学习快捷键的工具。

Ø  **格式化代码**：格式化import列表**Ctrl+Alt+O**，格式化代码**Ctrl+Alt+L**。

Ø  **切换窗口**：**Alt+Num**，常用的有1-项目结构，3-搜索结果，4/5-运行调试。**Ctrl+Tab**切换标签页，**Ctrl+E/Ctrl+Shift+E**打开最近打开过的或编辑过的文件。

Ø  **单元测试**：**Ctrl+Alt+T**创建单元测试用例。

Ø  **运行**：**Alt+Shift+F10**运行程序，**Shift+F9**启动调试，**Ctrl+F2**停止。

Ø  **调试**：**F7/F8/F9**分别对应Step into，Step over，Continue。

---



psvm 新建public static void main(String[] args) {    }

ctrl+shift+alt+u 生成项目包依赖关系图 packages dependence relation

ctrl d 复制一行到下一行

自行添加快捷键->duplicate entire lines

ctrl shift / 多行注释/* ... */ 

ctrl x 删除该行、ctrl c 复制该行

ctrl f 当前文件种搜索关键词

ctrl shift f 整个工程种搜索关键词

Search Everywhere  Double Shirt 

ctrl g 跳转到哪一行

ctrl F12 快速导航，显示当前类的成员的列表

alt 向上箭头/向下箭头 跳至该文件内的不同方法

Find Usages  Alt + F7  查找类或方法的使用

ctrl n 搜索文件

ctrl r 替换

选中 ctrl  - 缩为一行

ctrl shift j 连接join两行为一行，删除不必要的空间

crtl F4 关闭文件

ctrl+click 进入该类的源码声明

alt enter 自动导包、在implement后，查看需要实现的方法进行自动补入

Recent Files  Ctrl+E 



Alt + Insert(Code | Generate)

>在编辑器中使用Alt + Insert(代码|生成),您可以轻松地为任何字段生成getter和setter方法的类。、toString方法



Shift + F6 (Refactor|Rename)

> 将光标放在你想重命名的变量,并按Shift + F6。在弹出窗口中输入新名字,或选择一个建议的名称,然后按回车键。 



Ctrl + O(Code|Override Methods)、Ctrl + I(Code|Implement methods)、

> 覆盖基类的方法通过按Ctrl + O(代码|覆盖方法)。——查看有哪些方法需要覆盖
>
> 接口的实现方法,当前类实现(或抽象基类的),使用Ctrl + I(代码|实现方法)。  ——查看有哪些方法需要实现



Ctrl + Alt + B

> 将光标放在其使用的位置或其名称处，按Ctrl + Alt + B，导航到一个抽象的实现(s)方法



ctrl f9 重新编译修改后的代码





## IDEA 提示窗口

- 在代码中Ctrl+d

> 可以进行复制到下一行

- To compare two jar files, select one or both of them in the Project view and press Ctrl+D.

> 要比较两个jar文件，请在项目"View"中选择其中一个或两个，然后按Ctrl + D

- You can exclude any file from your project. As a result, such a file will be ignored by indexing, inspection and code completion. In the Project tool window, select the file you want to ignore, and choose Mark as plain text in its context menu.

> 可以从项目中排除任何文件。 因此，索引，检查和代码完成将忽略此类文件。在项目“Tools”窗口中，选择要忽略的文件，然后在其上下文菜单中选择“Mark as plain text”

- The keyboard shortcut Ctrl+K enables you to quickly invoke the Commit Changes dialog. This dialog shows all modifications in project, gives summary information of file status and suggests improvements before check-in.

> 使用键盘快捷键Ctrl + K可以快速调用“提交更改”对话框。
> 此对话框显示项目中的所有修改，提供文件状态的摘要信息，并在"check-in"前建议改进。

- It is very easy to toggle between find and replace functionality. When you perform search and replace in a file, pressing Ctrl+F shows the search pane. Pressing Ctrl+R adds field, where you can type the replace string. While in the Find in Path dialog, you can switch to replace by pressing Ctrl+Shift+R. Same way, press Ctrl+Shift+F to hide the Replace with field, and switch to mere search.

> 在查找和替换功能之间切换非常容易。在文件中执行搜索和替换时，按Ctrl + F将显示搜索窗格。 按Ctrl + R会添加字段，您可以在其中键入替换字符串。
> 在“在路径中查找”对话框中，您可以通过按Ctrl + Shift + R切换到替换。 同样，按Ctrl + Shift + F隐藏替换为字段，然后切换到仅搜索。

- If you are working on a large project, with numerous TODO items, filter them by scopes. Use the Scope-Based tab in the TODO tool window to show only those items that pertain to the scope of interest.

> 如果您正在处理包含大量TODO项目的大型项目，请按范围过滤它们。使用TODO工具窗口中的“Scope-Based”仅显示与感兴趣的范围相关的项目。

- If a method signature has been changed, IntelliJ IDEA highlights the tags that ran out of sync with the documentation comment and suggests a quick fix

> 如果方法签名（javadoc）已更改，IntelliJ IDEA会突出显示与文档注释不同步的标记，并建议快速修复

```
/**
 * @param a
 * ...
 * /
```

> 会自动显示警告：Remove @param a

- If there are too many run/debug configurations of the same type, you can group them into folders, and thus distinguish them visually.

> 如果相同类型的运行/调试配置太多，您可以将它们分组到文件夹中，从而在视觉上区分它们。

```
Application
	New Folder
    	SmapleApp
        MyApp
```

- You can avoid escaping backslashes in your regular expressions. Start typing a regular expression, then press Alt+Enter and choose Edit RegExp. The regular expression opens in a separate tab in the editor, where you can type backslashes as is.All changes are synchronized with the original regular expression, and escapes are presented automatically. When ready, just press Esc to close the regular expression editor.

> 启动正则表达式编辑器
> 您可以避免在正则表达式中遗漏转义反斜杠操作。 在您开始键入正则表达式之后，按Alt + Enter并选择Edit RegExp。 正则表达式在编辑器的单独选项卡中打开，您可以在其中键入反斜杠。所有更改都与原始正则表达式同步，并自动显示转义。 准备好后，只需按Esc即可关闭正则表达式编辑器。

```
import java.util.regex.Pattern;
public class RegexSample {
	Pattern clear_stuff = Pattern.compile("[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{3,4}");
```

- Speed up HTML, XML or CSS development with [Emmet](https://emmet.io/).Enable this framework in the corresponding page of the Editor/Emmet node (Settings/Preferences):

> 使用Emmet加速HTML，XML或CSS开发。在"File/Setting/Editor/Emmet"的相应页面中启用此框架

- If you place the caret at certain symbol and press Ctrl+Alt+Shift+T, you will see the list of refactorings applicable to the current context.

> 如果将插入符号放在某个符号上并按Ctrl + Alt + Shift + T，您将看到适用于当前上下文的重构列表。
> 重构包括：Rename、Change Signature、Convert To Instance Method...、Move...、Copy...、Safe Delete...

- Enable the horizontal scrolling with the mouse wheel by holding the Shift key.

> 按住Shift键，使用鼠标滚轮启用水平滚动。

- When using autopopup Code Completion, you can select the first item using Ctrl+句点. The selected name is automatically entered in the editor followed by dot.

> 使用autopopup Code Completion时，可以使用Ctrl + .选择第一个项目。 所选名称将自动输入编辑器，后跟 . 点。

- When an autopopup completion is active, Ctrl+向下箭头 and Ctrl+向上箭头 will close it and move the caret down or up in the editor.

> 当autopopup完成激活时，Ctrl +向下箭头和Ctrl +向上箭头将关闭它并在编辑器中向下或向上移动插入符号。





## Maven使用

### maven介绍

相对于传统的项目，Maven 下管理和构建的项目真的非常好用和简单，所以这里也强调下，尽量使用此类工具进行项目构建。

学习本讲还有一个前提：你必须会 Maven 相关知识点，Maven 相关知识点是不在本专题的讲解范围里面的，所以请自己私下进行学习。如果愿意你也可以看我过去整理的一份视频（提取码：wh5g）：<http://pan.baidu.com/s/1eSovBkI>



> 在 IDEA 使用 maven，需要在下载maven到主机，并设置好系统PATH参数

一般情况下，安装好的maven会自动生成C:\Users\username\ .m2，在IDEA中设置maven时，可以指定maven的settings.xml位置（C:\Users\username\ .m2\settings.xml）和本地仓库位置（C:\maven\my_local_repository）



注意，在 Maven 导入依赖包的时候，可以设置是否自动下载源码和文档。默认是没有勾选的，**也不建议勾选**，原因是这样可以加快项目从外网导入依赖包的速度，如果我们需要源码和文档的时候我们到时候再针对某个依赖包进行联网下载即可。IntelliJ IDEA 支持直接从公网下载源码和文档的。 

另外，根据已有的 Maven 骨架进行 Java Web 项目创建。其中需要特别注意的是，在创建项目过程中 Maven 会去外网中央仓库中下载对应的依赖或是组件，这个过程根据自身网络环境决定其快慢。如果出现无法下载的情况请自备 VPN 或者通过修改 Maven 配置文件 `settings.xml` 切换国内的中央仓库。 



### 使用maven组件来管理项目

了解了如何通过 Maven 骨架生成并启动一个最简单的 Java Web 项目，可是我们还是使用了 IntelliJ IDEA 的项目管理功能，来进行 Maven 项目的管理和构建。

一般 Maven 的项目，我们都可以**脱离 IntelliJ IDEA 的项目配置功能**，进行项目的独立的管理和构建。

/view/tools button/maven projects

1、双击使用生命周期命令进行clean编译、package打包

2、右键使用tomcat插件，运行项目



注意，虽然我们可以通过 Maven 组件进行项目的管理，但是这并不等同于我们可以完全抛弃 IntelliJ IDEA 的项目设置，比如我们在 `pom.xml` 文件中设置了 JDK 编译版本是 1.7，但是在项目的 `Ctrl + Shift + Alt + S` 配置中，我们配置的 JDK 是 1.8，那即使我们用 Maven 的编译工具或命令进行编译也是会调用 1.8的。还有即使我们在 `Ctrl + Shift + Alt + S` 配置中没有配置 Artifacts，在我们运行 Maven 的 Tomcat 插件的时候也会自动帮我们生成的。



## [极客学院 IDEA 使用](http://wiki.jikexueyuan.com/project/intellij-idea-tutorial/)

---

### /view

打开 toolbar 和  tool buttons



### ctrl+alt+s

打开settings配置



### settings/editor/color scheme/general

设置代码编辑时显示样式



### settings/editor/file encoding

设置编码，一般建议都改为utf-8，对于 `Properties` 文件，重要属性 `Transparent native-to-ascii conversion` 主要用于转换 `ascii`，一般都要勾选，不然 `Properties` 文件中的注释显示的都不会是中文。 



### source root，源目录和package，包目录和directory，普通目录的区别

源目录的作用标记该目录下的文件是可编译的，就是用来专门放 Java 类文件，相对于编译出来的 class 文件而言，它就是源。IntelliJ IDEA 支持对任意目录进行设置为 `Source root` 



### 对于压缩过的 JavaScript 文件，图标会有 `010` 图案。

比如 `jquery-1.10.1.min.js`



### 清除缓存和索引 file/invalidate caches

IntelliJ IDEA 的缓存和索引（首次加载项目的时候，都会创建索引 ）主要是用来加快文件查询，从而加快各种查找、代码提示等操作的速度

但是，IntelliJ IDEA 的索引和缓存并不是一直会良好地支持 IntelliJ IDEA 的，这某些特殊条件下，IntelliJ IDEA 的缓存和索引文件也是会损坏的，比如断电、蓝屏引起的强制关机，当你重新打开 IntelliJ IDEA，基本上百分八十的可能 IntelliJ IDEA 都会报各种莫名其妙错误，甚至项目打不开 

解决：一般建议点击 `Invalidate and Restart`，这样会比较干净。

有一个需要提醒的是：清除索引和缓存会使得 IntelliJ IDEA 的 `Local History`丢失，所以如果你项目没有加入到`版本控制`，而你又需要你项目文件的历史更改记录，那你最好备份下你的 `LocalHistory` 目录。

>tips：所有打开的项目大小加起来可能不到 5M，但是它们创建的索引大家就已经上百兆了。所以如果你 C 盘空间不足的情况下，最好转移下 `system` 目录 



### /build 编译

/run/debug configurations 中，例如 tomcat 或者 Spring Boot 配置中，`before launch:  build, activate tool window`，表示IntelliJ IDEA 默认在运行 tomcat 之前会先进行 `build` 操作。 



### /settings/build, execution, deployment/compiler/java compiler

IntelliJ IDEA 支持常见的几种编译器：`Javac`、`Eclipse`、`Ajc` 等。默认是 `Javac`，也推荐使用 `Javac` 



### Ctrl + Shift + Alt + S 项目结构设置

IntelliJ IDEA 支持 6 种 SDK。最常用的就是 `JDK` 和 `Mobile SDK`，其中在创建的时候如果没有先配置一个 `JDK` 的话，IntelliJ IDEA 则会提示你要先配置一个 `JDK`，然后才能配置 `Mobile SDK`。 

>在开发 Java 项目过程中，由于 IntelliJ IDEA 支持管理多个 `JDK`，所以你完全不用担心你系统上不同项目需要不同 `JDK`。 



### /project settings/project/

project language level：This language level is default for all project modules. A module specific language level can be configured for each of the modules as required.

作用：限定项目编译检查时最低要求的 JDK 特性。 假设我们有一个项目代码使用的 JDK 8 新特性：`lambda` 语法，但是 JDK 选择的却是 JDK 7，即使 `language level` 选择了 `8 - Lambdas，type annotation etc.`，也是没有多大意义的，一样会编译报错。 当我们项目使用的是 JDK 8，但是代码却没有使用 JDK 8 的新特性，最多使用了 JDK 7 的特性的时候我们可以选择 `7 - Diamonds，ARM，multi-catch etc.`。 



### .idea(directory based) 和 .ipr(file based)

创建项目的时候自动创建一个 `.idea` 的项目配置目录，来保存项目 `project` 的配置信息。这是默认选项。 

创建项目的时候自动创建一个 `.ipr` 的项目配置文件来保存项目的配置信息。 



> IntelliJ IDEA 项目的很多配置变动，都是以这些 XML 文件的方式来表现的，所以我们也可以通过了解这些 XML 文件，来了解 IntelliJ IDEA 的一些配置。
>
> 也因为此特性，所以如果在项目协同中，我们要保证所有的项目配置一致，就可以考虑把这些配置文件上传到版本控制中（包括 `.idea` 目录和 `.iml` 文件）。
>
> 如果把这些文件加入到`版本控制`之后，那又有一点是需要考虑的，那就是协同开发者 Checkout 项目下来之后，并且按自己的需求进行项目配置的之后，项目的 XML 文件也会跟着变化。此时，协同开发者的这些变化的文件，就不应该再上传到版本控制中，至于如何管理这些文件，可以在版本控制可以进行了解。



### .iml

`module` 的配置文件，而 .idea 是 project 的配置目录



### 新建包目录package

在没有文件的情况下包目录默认是连在一起的，这不方便看目录层级关系。 

解决：点击齿轮，在弹出的菜单中去掉选择标注 2 选项：`Compact Empty Middle Packages` 和 `hide Empty Middle Packages`



### out 目录

在项目被编译运行之后，会形成的一个文件夹



### 版本控制 version control，比如git、GitHub

对于 IDE 来讲，集成版本控制的 idea 本身就是它最大的亮点之一，很多开发者也是为此才使用它。 

IntelliJ IDEA 虽然是自带对一些版本控制工具（比如SVN和GIT）的支持插件，但是该装什么版本控制客户端还是要照样装的。 

IDEA 默认支持目前主流的版本控制软件：CVS、SVN、Git等，并因为目前太多人使用 Github 进行协同或是项目版本管理，所以 IntelliJ IDEA 同时自带了 Github 插件，方便 Checkout 和管理你的 Github 项目。

 

### Git 和 GitHub 的配置

要在 IntelliJ IDEA 中使用 Git，需要先安装 Git 客户端，这里推荐安装官网版本。 

在 /settings/version control 配置git（选择VCS）和GitHub（登录）



### 菜单栏上的GitHub

1. /VCS/checkout from version control/GitHub

支持直接从登录的GitHub账号上checkout项目

2. /VCS/import into version control/share project on GitHub

支持把当前项目分享到GitHub账号上

3. 右键项目，/create Gist.../

支持创建Gist（Instantly share code, notes, and snippets.）



### toolbar工具栏 上 git 操作按钮

第一个按钮：`Update Project` 更新项目。

第二个按钮：`Commit changes` 提交项目上所有变化文件。点击这个按钮不会立马提交所有文件，而是先弹出一个被修改文件的一个汇总框，具体操作下面会有图片进行专门介绍。

第三个按钮：`Compare with the Same Repository Version` 当前文件与服务器上该文件通版本的内容进行比较。如果当前编辑的文件没有修改，则是灰色不可点击。

第四个按钮：`Show history` 显示当前文件的历史记录。

第五个按钮：`Revert` 还原当前被修改的文件到未被修改的版本状态下。如果当前编辑的文件没有修改，则是灰色不可点击。



### 菜单栏上git版本控制操作区

/VCS/Git/



### /settings/version control/ignored files

，对于不想加入到版本控制的文件，可以添加要此忽略的列表中。但是如果已经加入到版本控制的文件使用此功能，则表示该文件 或 目录无法再使用版本控制相关的操作，比如提交、更新等。 



### git commit changes的change list

`Move to Another Changelist` 将选中的文件转移到其他的 `Change list` 中。

`Change list` 是一个重要的概念，这里需要进行重点说明。

> 很多时候，我们开发一个项目同时并发的任务可能有很多，每个任务涉及到的文件可能都是基于业务来讲的。所以就会存在一个这样的情况：我改了 30 个文件，其中 15 个文件是属于订单问题，剩下 15 个是会员问题，那我希望提交代码的时候是根据业务区分这些文件的，这样我填写 `Commit Message` 是好描述的，同时在文件多的情况下，我也好区分这些要提交的文件业务模块。所以我一般会把属于订单的 15 个文件转移到其他的 `Change list`中，先把专注点集中在 15 个会员问题的文件，先提交会员问题的 `Change list`，然后在提交订单会员的 `Change list`。我个人还有一种用法是把一些文件暂时不提交的文件转移到一个我指定的 `Change list`，等后面我觉得有必要提交了，再做提交操作，这样这些文件就不会干扰我当前修改的文件提交。总结下 `Change list` 的功能就是为了让你更好地管理你的版本控制文件，让你的专注点得到更好的集中，从而提升效率。



### 实时代码模板 live templates

/idea_config/config/templates

实时代码模板本质是用 XML 文件来保存的，所以传播自己的实时代码模板只要传播对应的文件即可。 IntelliJ IDEA 的实时代码模板保存在 `/templates` 目录下 

实时代码模板，其实只是为了让我们更加高效的写一些固定模式的代码，以提高编码效率，同时也可以增加个性化。 



> 调用常规的实时代码模板主要是通过两个快捷键：`Tab` 和 `Ctrl + J` 

在输入 `sys` 后按 `Tab` 键，即立即生成预设语句。

![实时代码模板的介绍](http://wiki.jikexueyuan.com/project/intellij-idea-tutorial/images/xvii-a-live-templates-introduce-1.gif)

如果按 `Ctrl + J` 则会先提示与之匹配的实时代码模板介绍，然后还需按 `Enter` 才可完成预设语句的生成。 



### 文件代码模板 

/settings/editor/file and code templates

我们在项目中创建某些类型文件时，就已经在对应这些新文件中预设了代码内容。 

IntelliJ IDEA 默认新建类**自带的类注释格式**一般不够友好或是规范，所以我们一般需要自己根据公司编码规范进行设置。 

另外，我们可以衍生出很多玩法，比如：我们的项目 Controller、Service、Dao 等常用新对象，都是要各自**继承某个类、实现某些接口或预设某些方法**，也都可以通过这样的文件代码模板来实现。 

1. Class：设置includes中的header内容

```
/**
 * Created by ${USER} on ${DATE}
 */
```

2. 新建XML

IntelliJ IDEA 默认是没有提供 XML 文件的创建的，自行添加my XML.xml

3. 设置自定义变量

当我们需要用到一个固定值的自定义变量的时候并且该变量多个地方被引用，我们可以通过 VTL 语法的 `#set( $变量名 = "变量值内容" );` 来设置。 



### Emmet

/settings/editor/emmet/

Emmet 一般前端工程师用得比较多 

IntelliJ IDEA 自带 Emmet 功能，使用的快捷键是 `Tab`。

```html
ul>li*4>a
ul#id>li*4>a
ul#id>li.class$*4>a
ul#id>li.class$*4>a{Content $}
ul#nav>li.item$*4>a{Item $}
# 使用 tab 生成
<ul id="nav">
    <li class="item1"><a href="">Item 1</a></li>
    <li class="item2"><a href="">Item 2</a></li>
    <li class="item3"><a href="">Item 3</a></li>
    <li class="item4"><a href="">Item 4</a></li>
</ul>
```



/settings/editor/emmet/css IntelliJ IDEA 支持主流四个浏览器内核的一些特别 CSS 书写

```css
.myclass{
    box-pack
}
》》》tab
.myclass{
    -webkit-box-pack: ;
    -moz-box-pack: ;
    -ms-box-pack: ;
    box-pack: ;
}
```



### 代码模板postfix completion

/settings/editor/general/postfix completion

使用：

非空的判断在 Java 代码中应该是非常常见的一句话代码，如果用 Live Templates 当然也是可以快速生成，但是没有上图 Gif 这种 Postfix Completion 效果快。 

```java
args[1].null

        if (args[1] == null) {

        }
```



### 插件的使用

IntelliJ IDEA 本身很多功能也都是通过插件的方式来实现的，只是 IntelliJ IDEA 本身就是它自己的插件平台最大的开发者而已，开发了很多优秀的插件。 

/settings/plugins

- `All plugins` 显示所有插件。
- `Enabled` 显示当前所有已经启用的插件。
- `Disabled` 显示当期那所有已经禁用的插件。
- `Bundled` 显示所有 IntelliJ IDEA 自带的插件。
- `Custom` 显示所有我们自行安装的插件，如果你自己装了很多次插件的话，这个选项会用得比较多。



/settings/plugins/install jetbrains plugins：弹出 IntelliJ IDEA 公司自行开发的插件仓库列表，供下载安装。 

/settings/plugins/browse repositories：弹出官方插件仓库中所有插件列表供下载安装。 

/settings/plugins/install plugin from disk：浏览本地的插件文件进行安装，而不是从服务器上下载并安装。 

> 需要严重注意的是：在国内的网络下，很经常出现显示不了插件列表，或是显示了插件列表，无法下载完成安装。这时候请自行开VPN，一般都可以得到解决。



使用：

支持Markdown support for intellij products

翻译插件translation 使用alt+T

快捷键提示key promoter



### debug技巧

/settings/build, execution, deployment/debugger

 Debug 连接方式，默认是 `Socket`。`Shared memory` 是 Windows 特有的一个属性，一般在 Windows 系统下建议使用此设置，相对于 `Socket` 会快点。 



### Debug 常用快捷键

| 快捷键            | 介绍                                                         |
| ----------------- | ------------------------------------------------------------ |
| F7                | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中 `必备` |
| F8                | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则不进入当前方法体内 `必备` |
| F9                | 在 Debug 模式下，恢复程序运行，但是如果该断点下面代码还有断点则停在下一个断点上 `必备` |
| Alt + F8          | 在 Debug 的状态下，选中对象，弹出可输入计算表达式调试框，查看该输入内容的调试结果 `必备` |
| Ctrl + F8         | 在 Debug 模式下，设置光标当前行为断点，如果当前已经是断点则去掉断点 |
| Shift + F7        | 在 Debug 模式下，智能步入。断点所在行上有多个方法调用，会弹出进入哪个方法 |
| Shift + F8        | 在 Debug 模式下，跳出，表现出来的效果跟 `F9` 一样            |
| Ctrl + Shift + F8 | 在 Debug 模式下，指定断点进入条件                            |
| Alt + Shift + F7  | 在 Debug 模式下，进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果方法体还有方法，则会进入该内嵌的方法中，依此循环进入 |
| Drop Frame        | 这个不是一个快捷键，而是一个 Debug 面板上的按钮。该按钮可以用来退回到当前停住的断点的上一层方法上，可以让过掉的断点重新来过 |

有时候我们可以这样粗鲁地认为 Debug 的使用就是等同于这几个快捷键的使用，所以上面的 `必备` 快捷键是我们必须牢记的，这些也是开发很常用的。



### 数据库管理工具

很多人认为在 IDEA 配置 Database 就是为了有一个 GUI 管理数据库功能，但是这并不是 IntelliJ IDEA 的 Database 最重要特性。数据库的 GUI 工具有很多，IntelliJ IDEA 的 Database 也没有太明显的优势。



IntelliJ IDEA 的　Database 最大特性：

对于 Java Web 项目来讲，常使用的 ORM 框架，如 **Hibernate**、**Mybatis** 有很好的支持，比如配置好了 Database 之后，IntelliJ IDEA 会自动识别 **domain 对象**与**数据表**的关系，也可以通过 Database 的数据表直接生成 domain 对象等等。 



完成：

一个完整的配置 Database 过程，对于数据库需要的依赖包，IntelliJ IDEA 可以自动帮我们下载，所以我们只要配置对应的连接参数即可。 



Database 常用四个操作

1、同步当前数据库连接，这是最重要的操作，有一些情况下，当我们配置好连接之后，没有显示数据表，那就是需要点击该按钮进行同步。 还有一种情况就是我们在 IntelliJ IDEA 之外用其他工具操作数据库，比如新建表。而此时 IntelliJ IDEA 的 Database 如果没有同步到新表，也是需要点击此按钮进行同步的。 

2、配置当前连接，设置连接

3、断开当前连接

4、查看当前所选对象的图标结构，比如我们当前所选中的是整个数据库名，点击查看当前数据库下的所有数据表的图标结构图
