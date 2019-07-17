---
layout: post
title: React에서 Sass 경로 설정 (webpack)
excerpt: "React Sass 설정 관련"
categories: [React]
comments: true
---

# React에서 SASS파일을 불러올때 경로 설정.

webpack.config.js 오픈 후 sassRegex를 찾는다.

```js
{
    test: sassRegex,
    exclude: sassModuleRegex,
    use: getStyleLoaders(
    {
        importLoaders: 2,
        sourceMap: isEnvProduction && shouldUseSourceMap,
    },
    'sass-loader'
    ).concat({
    loader: require.resolve('sass-loader'),
    options: {
        includePaths: [paths.appSrc + '/styles'],
        sourceMap: isEnvProduction && shouldUseSourceMap
    }
    }),
    // Don't consider CSS imports dead code even if the
    // containing package claims to have no side effects.
    // Remove this when webpack adds a warning or an error for this.
    // See https://github.com/webpack/webpack/issues/6571
    sideEffects: true,
},
```

위와 같이 수정해 준 후 서버를 재시작 한다.

추가 SASS파일을 import할때 상대 경로 없이 styles 디렉토리 기준으로 파일을 불러올 수 있다.

```js
@import 'utils.scss';
```

추가 import조차 포함시키고 싶을때 sass-loader의 data 설정을 해주면 된다.

```js
{
    test: sassRegex,
    exclude: sassModuleRegex,
    use: getStyleLoaders({
    importLoaders: 2,
    sourceMap: isEnvProduction && shouldUseSourceMap
    }).concat({
    loader: require.resolve('sass-loader'),
    options: {
        includePaths: [paths.appSrc + '/styles'],
        sourceMap: isEnvProduction && shouldUseSourceMap,
        data: `@import 'utils';`
    }
    }),
    // Don't consider CSS imports dead code even if the
    // containing package claims to have no side effects.
    // Remove this when webpack adds a warning or an error for this.
    // See https://github.com/webpack/webpack/issues/6571
    sideEffects: true
},
```

위 설정을 적용하면 모든 SASS파일에서 utils를 사용할 수 있다.

