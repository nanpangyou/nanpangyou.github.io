---
title: 使用karma-mocha-sinon和travis尝试做持续测试
date: 2019-12-04 20:31:52
---

1. Karma（[ˈkɑrmə] 卡玛）是一个测试运行器，它可以呼起浏览器，加载测试脚本，然后运行测试用例
2. Mocha（[ˈmoʊkə] 摩卡）是一个单元测试框架/库，它可以用来写测试用例
3. Sinon（西农）是一个 spy / stub / mock 库，用以辅助测试
4. travis 是一个免费的 CI 平台

# 安装各种工具

```
npm i -D karma karma-chrome-launcher karma-mocha karma-sinon-chai mocha sinon sinon-chai karma-chai karma-chai-spies
```

# 在根目录里创建`karma.config.js`

`karma.config.js`中主要制定了测试的浏览器。带测试的文件（file 项下的配置）

```
// 新建 karma.conf.js，内容如下
module.exports = function(config) {
  config.set({
    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: "",
    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ["mocha", "sinon-chai"],
    client: {
      chai: {
        includeStack: true
      }
    },

    // list of files / patterns to load in the browser
    //   **/*.js 代表循环该目录下的所有js文件
    files: ["dist/**/*.js", "dist/**/*.css"],

    // list of files / patterns to exclude
    exclude: [],

    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {},

    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ["progress"],

    // web server port
    port: 9876,

    // enable / disable colors in the output (reporters and logs)
    colors: true,

    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,

    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,

    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ["ChromeHeadless"],

    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false,

    // Concurrency level
    // how many browser should be started simultaneous
    concurrency: Infinity
  });
};

```

# 创建 `test` 文件夹，并且在文件夹下创建`xxx.test.js`

```
//这里我做的是一个按钮的单元测试，所以新建button.text.js
import Vue from "vue";
import Button from "../src/button";

Vue.config.productionTip = false;
Vue.config.devtools = false;
const expect = chai.expect;

describe("Button", () => {
  it("存在.", () => {
    expect(Button).to.be.ok;
  });
  it("可以设置icon.", () => {
    const Constructor = Vue.extend(Button);
    const vm = new Constructor({
      propsData: {
        icon: "setting"
      }
    }).$mount();
    const useElement = vm.$el.querySelector("use");
    expect(useElement.getAttribute("xlink:href")).to.equal("#i-setting");
    vm.$destroy();
  });
  it("可以设置loading.", () => {
    const Constructor = Vue.extend(Button);
    const vm = new Constructor({
      propsData: {
        icon: "setting",
        loading: true
      }
    }).$mount();
    const useElements = vm.$el.querySelectorAll("use");
    expect(useElements.length).to.equal(1);
    expect(useElements[0].getAttribute("xlink:href")).to.equal("#i-loading");
    vm.$destroy();
  });
  it("icon 默认的 order 是 1", () => {
    const div = document.createElement("div");
    document.body.appendChild(div);
    const Constructor = Vue.extend(Button);
    const vm = new Constructor({
      propsData: {
        icon: "settings"
      }
    }).$mount(div);
    const icon = vm.$el.querySelector("svg");
    expect(getComputedStyle(icon).order).to.eq("1");
    vm.$el.remove();
    vm.$destroy();
  });
  it("设置 iconPosition 可以改变 order", () => {
    const div = document.createElement("div");
    document.body.appendChild(div);
    const Constructor = Vue.extend(Button);
    const vm = new Constructor({
      propsData: {
        icon: "setting",
        iconPosition: "right"
      }
    }).$mount(div);
    const icon = vm.$el.querySelector("svg");
    expect(getComputedStyle(icon).order).to.eq("2");
    vm.$el.remove();
    vm.$destroy();
  });
  it("点击 button 触发 click 事件", () => {
    const Constructor = Vue.extend(Button);
    const vm = new Constructor({
      propsData: {
        icon: "setting"
      }
    }).$mount();
    //判断callback是否被执行
    const callback = sinon.fake();
    vm.$on("click", callback);
    vm.$el.click();
    expect(callback).to.have.been.called;
    vm.$destroy();
  });
});

```

该文件主要写了单元测试的语句，使用 mocha 来完成测试的，通过描述 Button 的不同动作，使用 it 函数来包裹每一个动作并在内部写断言
这里主要使用了 chai.js 的断言库还有 sinon 的 fake 函数来充当 mock 函数

# 在`package.json`中写测试调用语句

改写`scripts`
添加

```
{
  "dev-test": "parcel watch test/* --no-cache & karma start",
  "test": "parcel build test/* --no-cache --no-minify && karma start --single-run"
}
```

至此 就可以在开发过程中进行单元测试了，如果需要进行一次单元测试，则`npm run test`,如果需要 watch 打包后的文件，则`npm run dev-test`

# 使用 travis CI 来在每次 push 代码后自动测试

在根目录下创建`.travis.yml`文件

```
language: node_js
node_js:
  - "8"
  - "9"
  - "10"
addons:
  chrome: stable
sudo: required
before_script:
  - "sudo chown root /opt/google/chrome/chrome-sandbox"
  - "sudo chmod 4755 /opt/google/chrome/chrome-sandbox"
```

打开[travis](https://travis-ci.com/)的官网
添加你的 git 仓库当你每次进行`git push`的时候，就会自动进行`npm test`将测试结果发送至你的邮箱
