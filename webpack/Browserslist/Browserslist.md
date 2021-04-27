# Browserslist

browserslist 是在不同的前端工具之间共用目标浏览器和 node 版本的配置工具。它主要被以下工具使用：

- [Autoprefixer](https://github.com/postcss/autoprefixer)
- [Babel](https://github.com/babel/babel/tree/master/packages/babel-preset-env)
- [post-preset-env](https://github.com/jonathantneal/postcss-preset-env)
- [eslint-plugin-compat](https://github.com/amilajack/eslint-plugin-compat)
- [stylelint-unsupported-browser-features](https://github.com/ismay/stylelint-no-unsupported-browser-features)
- [postcss-normalize](https://github.com/jonathantneal/postcss-normalize)
- [obsolete-webpack-plugin](https://github.com/ElemeFE/obsolete-webpack-plugin)

[browserslist示例](https://github.com/browserslist/browserslist-example) 演示了上面列举的每个工具是如何使用 browserslist 的。所有的工具将自动的查找当前工程规划的目标浏览器范围，前提是你在前端工程的 package.json 里面增加如下配置：

```json
{
  "browserslist": [
    "last 1 version",
    "> 1%",
    "maintained node versions",
    "not dead"
  ]
}
```

或者在工程的根目录下存在`.browerslistrc`配置文件（内容如下）：

```bash
# 注释是这样写的，以#号开头
last 1 version
> 1%
maintained node versions
not dead
```

开发者通过设置浏览器版本查询范围(eg. `last 2 versions`)，使得开发者不用再频繁的手动更新浏览器版本。browserslist 使用 [Can I Use](http://caniuse.com/) 网站的数据来查询浏览器版本范围。

## browerslist 衍生的工具

- browserslit-ga： 该工具能生成访问你运营的网站的浏览器的版本分布数据，以便用于类似`> 0.5% in my stats`查询条件, 前提是你运营的网站部署有 Google Analytics。
- browserslist-useragent ： 检验 某浏览器的user-agent 字符串是否匹配 browserslist 给出的浏览器范围。
- browserslist-useragent-ruby ： 功能同上，ruby 库。
- caniuse-api： 返回支持指定特性的浏览器版本范围
- npx browserslist ：在前端工程目录下运行上面命令，输出当前工程的目标浏览器列表。

## 查询来源

browerslist 将使用如下配置文件限定的的浏览器和 node 版本范围：

1. 工具 options，例如 Autoprefixer 工具配置中的 browsers 属性。

2. `BROWERSLIST` 环境变量。

3. 当前目录或者上级目录的`browserslist`配置文件。

4. 当前目录或者上级目录的`browserslistrc`配置文件。

5. 当前目录或者上级目录的`package.json`配置文件里面的`browserslist`配置项（推荐）。

6. 如果上述的配置文件缺失或者其他因素导致未能生成有效的配置，browserslist 将使用默认配置。

   ```bash
   > 0.5%, last 2 versions, Firefox ESR, not dead
   ```

## 最佳实践

1.仅仅当你在特定浏览器上开发类似于信息亭之类的 web app 的时候，才可以用类似`last 2 Chrome versions`的查询条件来锁定特别具体的浏览器品牌和版本。市面上有各种各样的浏览器，同时浏览器的版本碎片化也很严重，如果你在开发一款通用的 webapp，那就应该考虑浏览器多样性导致的兼容问题。

2.如果你不想用 browserslsit 的默认设置，推荐使用`last 1 version`, `not dead` 和 `> 0.2%`(或者`> 1% in US`,`> 1% in my stats`).仅仅使用`last n versions` 将添加太多的废弃浏览器到工程里面来，同时也并没有有效的覆盖那些占有率仍然很高的老版本浏览器

3.不移除某些浏览器，是因为你不了解它们的分布。Opera mini 在非洲有一亿用户，全球范围内，它也比 微软的 Edge 浏览器更加流行。QQ 浏览器的使用量比桌面端的火狐和 Safari 浏览器加起来还多。

## 查询条件列表

- `> 5%`: 基于全球使用率统计而选择的浏览器版本范围。`>=`,`<`,`<=`同样适用。
- `> 5% in US` : 同上，只是使用地区变为美国。支持两个字母的国家码来指定地区。
- `> 5% in alt-AS` : 同上，只是使用地区变为亚洲所有国家。[这里](https://github.com/ben-eb/caniuse-lite/tree/master/data/regions)列举了所有的地区码。
- `> 5% in my stats` : 使用[定制的浏览器统计数据](https://github.com/browserslist/browserslist#custom-usage-data)。
- `cover 99.5%` : 使用率总和为99.5%的浏览器版本，前提是浏览器提供了使用覆盖率。
- `cover 99.5% in US` : 同上，只是限制了地域，支持两个字母的国家码。
- `cover 99.5% in my stats` :使用[定制的浏览器统计数据](https://github.com/browserslist/browserslist#custom-usage-data)。
- `maintained node versions` :所有还被 node 基金会维护的 node 版本。
- `node 10` and `node 10.4` : 最新的 node 10.x.x 或者10.4.x 版本。
- `current node` :当前被 browserslist 使用的 node 版本。
- `extends browserslist-config-mycompany` :来自browserslist-config-mycompany包的查询设置
- `ie 6-8` : 选择一个浏览器的版本范围。
- `Firefox > 20` : 版本高于20的所有火狐浏览器版本。`>=`,`<`,`<=`同样适用。
- `ios 7` :ios 7自带的浏览器。
- `Firefox ESR` :最新的火狐 ESR（长期支持版） 版本的浏览器。
- `unreleased versions` or `unreleased Chrome versions` : alpha 和 beta 版本。
- `last 2 major versions` or `last 2 ios major versions` :最近的两个发行版，包括所有的次版本号和补丁版本号变更的浏览器版本。
- `since 2015` or `last 2 years` :自某个时间以来更新的版本（也可以写的更具体`since 2015-03`或者`since 2015-03-10`）
- `dead` :通过`last 2 versions`筛选的浏览器版本中，全球使用率低于0.5%并且官方声明不在维护或者事实上已经两年没有再更新的版本。目前符合条件的有 `IE10`,`IE_Mob 10`,`BlackBerry 10`,`BlackBerry 7`,`OperaMobile 12.1`。
- `last 2 versions` :每个浏览器最近的两个版本。
- `last 2 Chrome versions` :chrome 浏览器最近的两个版本。
- `defaults` :默认配置`> 0.5%, last 2 versions, Firefox ESR, not dead`。
- `not ie <= 8` : 浏览器范围的取反。
- 可以添加`not`在任和查询条件前面，表示取反



## Debug

直接在工程目录下运行`npx browserslist` 来查看你配置的筛选条件筛选出的浏览器版本范围。

```bash
$ npx browserslist

and_chr 61
and_ff 56
and_qq 1.2
and_uc 11.4
android 56
baidu 7.12
bb 10
chrome 62
edge 16
firefox 56
ios_saf 11
opera 48
safari 11
samsung 5


```



## 注意事项

Browserslist 会处理浏览器的每个版本，所以应该避免配置这样的查询条件`Firefox > 0`.

多个查询条件组和在一起之后，其之间的的覆盖是以`OR` 的方式，而是不是`AND`,也就是说只要浏览器版本符合筛选条件里面的一种即可。

所有的查询条件均基于[Can I Use](http://caniuse.com/)的支持列表。例如：`last 3 ios versions` 可能会返回`8.4, 9.2, 9.3`(混合了主版本和次版本)，然而`last 3 Chrome versions`可能返回`50, 49, 48`（只有主版本），总之一切以 CanIUse网站收集的浏览器版本数据为准。

## 浏览器分类（大小写不敏感）

- `Android` for Android WebView.
- `Baidu` for Baidu Browser.
- `BlackBerry` or `bb` for Blackberry browser.
- `Chrome` for Google Chrome.
- `Edge` for Microsoft Edge.
- `Electron` for Electron framework. It will be converted to Chrome version.
- `Explorer` or `ie` for Internet Explorer.
- `ExplorerMobile` or `ie_mob` for Internet Explorer Mobile.
- `Firefox` or `ff` for Mozilla Firefox.
- `FirefoxAndroid` or `and_ff` for Firefox for Android.
- `iOS` or `ios_saf` for iOS Safari.
- `Node` for Node.js.
- `Opera` for Opera.
- `OperaMini` or `op_mini` for Opera Mini.
- `OperaMobile` or `op_mob` for Opera Mobile.
- `QQAndroid` or `and_qq` for QQ Browser for Android.
- `Safari` for desktop Safari.
- `Samsung` for Samsung Internet.
- `UCAndroid` or `and_uc` for UC Browser for Android

## package.json

如果你想减少工程根目录下的配置文件的数量，可以在 `package.json` 中设置 `browserslist` 配置项，如下所示:

```json
{
  "private": true,
  "dependencies": {
    "autoprefixer": "^6.5.4"
  },
  "browserslist": [
    "last 1 version",
    "> 1%",
    "IE 10"
  ]
}

```

Browserslist配置文件应该被命名为 `.browserslistrc` 或者 `browserslist` 每条查询条件独占一行。 注释用 `#` 开头:

```bash
# Browsers that we support

last 1 version
> 1%
IE 10 # sorry
```

Browserslist 将检查`path`路径上每一级目录下面是否有配置文件. 所以，如果工具要处理的文件路径是这样的 `app/styles/main.css`, 那么你可以将配置文件放置在根目录, `app/` 或者 `app/styles`。

也可以在`BROWSERSLIST_CONFIG`环境变量中直接指定配置文件的路径 。

## Shareable Configs

可以使用如下写法，从另外一个输出 browserslist 配置的包导入配置数据:

```bash
  "browserslist": [
    "extends browserslist-config-mycompany"
  ]
```

为了安全起见，额外的配置包只支持前缀 `browserslist-config-` 的包命名.  npm包作用域也同样支持 `@scope/browserslist-config`,例如： `@scope/browserslist-config` or `@scope/browserslist-config-mycompany`.



## 环境的差异化配置

你可以为不同的环境配置不同的浏览器查询条件。 Browserslist 将依赖`BROWSERSLIST_ENV` 或者 `NODE_ENV`查询浏览器版本范围 . 如果两个环境变量都没有配置正确的查询条件，那么优先从 production 对应的配置项加载查询条件，如果再不行就应用默认配置。

In `package.json`:

```json
 "browserslist": {
    "production": [
      "> 1%",
      "ie 10"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version"
    ]
  }
```

In `.browserslistrc` config:

```bash
[production staging]
> 1%
ie 10

[development]
last 1 chrome version
last 1 firefox version
```

