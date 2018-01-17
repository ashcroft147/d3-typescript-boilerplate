# d3-typescript-boilerplate
This project is for the boiler-plate project of d3 with typescript

## Installing
nodejs가 기본적으로 설치되어 있는 환경에서 다음의 명령어를 통해 Typescript Compiler를 설치한다. 

~~~
$ npm install -g typescript
~~~

Typescript 의 설치가 완료되면, 다음의 명령어를 통해 설치가 정상적으로 수행되었는지 확인할 수 있다. 

~~~
$ tsc -v
~~~

Typescript를 webpack에 연동하기 위해서 다음과 같이 webpack을 설치한다. 

~~~
$ npm install webpack@latest -g
~~~

webpack의 Version Check 및 설치가능한 최신 버전을 조회하는 명령어는 다음과 같다. 

~~~
$ npm list webpack -g --설치 버전 Check
$ npm view webpack version --설치가능한 최신 webpack 버전
$ npm link webpack --webpack이 globally 설치된 경우 local에 연결
~~~

## tsconfig.json 작성
tsconfig.json은 TypeScript 프로젝트의 root 디렉토리에 위치하며, 반복적으로 사용되는 TypeScirpt의 Compiler Option을 정의하여 Compile 시, 해당파일의 Option 및 설정을 참조한다. 

~~~
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "sourceMap": true
    },
    "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules"
    ]
}
~~~

## webpack.config.js 작성
Typescript 사용을 위한 webpack.config.js 파일에 대한 설정은 다음과 같다. 
우선, webpack이 TypeScript를 Compile 및 Transpile 할 수 있도록 TypeScript Loader인 ts-loader를 설치한다.
ts-loader 설정을 통해 tsconfig.json을 읽어와 typescript compilation을 수행한다. 

~~~
$ npm install ts-loader --save-dev
~~~

tsconfig.json을 통해 compiler 옵션을 정의해 주고 난 후 
webpack.config.js를 다음과 같이 작성한다. 

아래와 같이 작성후 webpack을 실행하면 확장자가 .ts 및 .tsc인 파일들을 찾아내어 output 디렉토리에 bundle.js 파일을 생성한다. 

~~~
const path = require('path');

module.exports = {
  entry: './src/index.ts',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [ '.tsx', '.ts', '.js' ]
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};

~~~

### Inline sourcemap 파일의 생성
Debugging 작업에 유용하게 사용될 inline source map 파일을 생성하기 위해서는, 우선 tsconfig.json 파일에 "sourcemap":true 옵션을 지정한다. 그리고 source map 파일을 추출하여 bundle.js 파일에 엮어주기 위해서는 webpack.config.js 파일에 devtool: "in"

### Using D3 Library
D3를 사용하기 위해서는 D3를 설치한 후에 D3를 Typescript에서 인식하기 위해서 type definition을 다음과 같이 설치한다. 

~~~
$ npm install --save d3 -- d3 설치
$ npm install --save-dev @types/d3 -- d3 type definition 설치
~~~

### webpack-dev-server 설정하기
webpack-dev-server 는 간단한 web server를 사용할수 있게 해준다. 
다음과 같이 설치하자.

~~~
$ npm install --save-dev webpack-dev-server
~~~

그리고 webpack.config.js 파일에서 dev server가 참조해야할 file의 위치를 설정하는 property를 추가한다. 

~~~
devServer: {
  contentBase: './dist'
}
~~~

npm script 명령어를 추가한다. 

~~~
"start": "node ./node_modules/webpack-dev-server/bin/webpack-dev-server.js --open chrome"
~~~

위의 설정이 완료된 후 npm run start 명령어를 입력하면, webpack-dev-server가 실행된다. 