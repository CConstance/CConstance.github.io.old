---
layout:     post
title:      "一个技术文章阅读器"
subtitle:   " \"入门React Native\""
date:       2016-07-12 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-rn-1.jpg"
catalog: true
tags:
    - Technology
    - Engineering
    - App Development
---

>这只是一个非常简单的React-Native入门的小项目。其主要功能是每天推荐一些技术文章供阅读。这篇文章里只有非常简单的后台的介绍，主要是用于学习React-Native的网络请求、界面、语法的相关知识。

### 何为React-Native

我们手机上的app大致可以分为两类，一类是Native App；一类是Web App。 

所谓Native App就是使用原生组件来开发的应用，在Android上就是我们使用Java技术栈来开发，iOS上就是对应的Objective-C或者Swift语言来开发。其好处在于性能好，速度快，可以非常方便地调用系统底层的组件。但也存在着上架流程慢，更新效率低（有过App Store上架惊艳的同学可能深有体会），开发速度慢，多版本维护非常痛苦的缺点。

另一类就是Web App。即在App中使用Web开发的方式，例如HTML、CSS、JavaScript语言来进行开发，最后使用Webview将网页封装成一个app。这种技术就恰恰好弥补了Native App的缺点。它只需要一次上架，之后我们可以在服务端更新我们的网页，就可以实现热更新；同时，我们只需要开发一个版本，就可以适应iOS、Android多平台，甚至WP也可以使用。但是它的缺点也是非常明显的，移动设备的性能和电力大大制约了Webview的发展。很多js技术在移动端浏览器上都不原生支持，移动端浏览器对不同技术的不同实现方式也导致了很多网页显示效果的迥异。

![](http://ww3.sinaimg.cn/large/651b652ejw1f5thau7601j20vq0evag6.jpg)

以Fetch为例，大部分桌面的浏览器都早已支持这种XmlHTTPRequest的替代，然而在移动端的支持还不是非常好。iOS全线不支持。

所以很正常的一个想法就是，有没有可能将两种技术混合，叫做Hybrid App，它可以集Native和Web的优点于一身，还可以互相不短，似乎可以成为未来的一个趋势。但是市面上有很多Hybrid的方案，如PhoneGap、Cordova等等，所以大家的一个主要的顾虑就是标准化和成熟度上。

而React Native则普遍被大家接受。它抛弃了Web组件(即HTML+CSS+JavaScript的组件)，而是调用了原生的组件（以此来解决性能问题）。它的底层是JavaScript Core，可以使用Web的方式来进行管理，既可以做到高效开发，也可以做到快速部署和热更新。性能也相对比较高。

### 一个简单的技术文章阅读器

![](https://ws4.sinaimg.cn/large/651b652ejw1f5thywuvjlj204a0bnjrk.jpg)

上图是我们的技术文章阅读器的主要技术框架。用户由index.android.js和index.ios.js导向TechBuilds/index.js文件。index.js文件中主要实现了一个导航器Navigator的对象，之后页面之间的跳转都通过这个Navigator来实现。同时也实现了一个”全局”的Navigation Bar 用于在所有页面之间展示，从而实现后退的效果等。将来的TabBar也会通过类似的方式来实现。

这个Navigator可以设定initialScene，也就是初始的页面，在这里我们设定其为articles中的列表元素。

在articles.js中就是我们的主要页面之列表页。

![](https://ws1.sinaimg.cn/large/651b652ejw1f5ti6tmmd4j20di0ju76m.jpg)

#### 详解列表页

```
class ArticleCell extends React.Component {
  props: {
    title: string;
    link:string;
    onPress: ?() => void;
    style: any;
  };

  render() {
    var title = this.props.title;
    var link = this.props.link;
    var tick;
    var cell =
      <View style={[styles.cell, this.props.style]}>
        <View style={styles.titleSection}>
          <Text numberOfLines={2} style={styles.titleText}>
            {title}
          </Text>
        </View>
      </View>;

    if (this.props.onPress) {
      cell =
        <ZtTouchable onPress={this.props.onPress}>
          {cell}
        </ZtTouchable>;
    }

    return cell;
  }
}
```

列表页中的每一个元素，被称之为ArticleCell，存在于ArticleCell.js中，其作用是展示一行技术文章的标题，并给他绑定onPress操作。

之后在Articles.js中，首先通过fetch方法来取得这些文章：

```
fetchData() {
    fetch('http://10.10.1.47:6789/article')
        .then((response) => response.json())
        .then((responseData) => {
            this.setState({
            articles: responseData.articles,
        });
    }).done();
}
```

fetch到的Data存放在this.state.articles中，如果articles为空，则渲染一个简单的文本框，标记Loading，否则，渲染出这个列表。

```
renderLoadingView() {
    return (
        <Text>
          正在加载数据……
        </Text>
    );
  }
render () {
    if (!this.state.articles) {
        return this.renderLoadingView();
    }
    var list=this.state.articles;
    console.log(list);
    var result=[];
    for (var i in list){
        var row=(
            <ArticleCell
                style={[styles.container]}
                title={list[i].title}
                link={list[i].link}
                onPress={this._loadDetail.bind(this,list[i].link)}
            />
        )
        result.push(row);
    }
    return(  
        <ScrollView style={{marginTop: 50}}>
            {result}
        </ScrollView>
    );
};
```

对每一个ArticleCell绑定一个LoadDetail的方法。这个方法将通过navigator切换到Detail中，Detail是我们实现的另一个界面。

```
_loadDetail(tlink) {
        this.props.navigator.push({
            component: Detail,
            name:'detail',
            params:{
                link:tlink
            }
        });
    };
```

#### 详情页

详情页的实现非常简单，我们使用一个webview来包裹这个这个网页就可以了。

```
class DetailView extends Component {
    props: {
    link:string;
    style: any;
  };
  constructor(props) {
		super(props);
	}
  render() {
    return (
        <WebView
        source={{uri: this.props.link}}
        style={{marginTop: 20}}
      />
    );
  }
}
```
效果图

![](http://ww2.sinaimg.cn/large/651b652ejw1f5tla40cdij20cf0i9ju7.jpg)

[Github地址](https://github.com/stevefermi/TechBuilds)