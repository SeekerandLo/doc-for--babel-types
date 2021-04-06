# doc-for-@babel/types

## 数组相关
1. **isArrayExpression**
```js
// 判断是不是数组表达式
path.isArrayExpression()


// code
const array = [1, 2, 3];


// use babel
const ast = parser.parse(code);
traverse(ast, {
    enter(path, state) {
        console.log(path.isArrayExpression());
    }
})


// console
Program false
VariableDeclaration false
VariableDeclarator false
Identifier false
ArrayExpression true
NumericLiteral false
NumericLiteral false
NumericLiteral false
```

2. **assertArrayExpression**
```js
// 断言使用
types.assertArrayExpression()

如果type不是ArrayExpression会报错
```

3. **arrayExpression**
```js
// 创建数组ast使用
types.arrayExpression(elements)

// use babel
// 创建一个数组ast： [“1”, “2"]
const arrayAst = types.arrayExpression([types.stringLiteral('1'), types.stringLiteral('2')]);

// 声明两个数组并且赋值
const dast = types.variableDeclaration('const', [types.variableDeclarator(types.identifier('array1'), arrayAst), types.variableDeclarator(types.identifier('array2'), arrayAst)]);

// 生成代码
const code = generate(dast);

// 结果
const array1 = ["1", "2"],
      array2 = ["1", "2"];
```

4. **arrayPattern**
```js
// 创建数组ast使用
types.arrayPattern(elements);

// 入参接受 PatternLike 类型
// PatternLike = Identifier | RestElement | AssignmentPattern | ArrayPattern | ObjectPattern;


// use babel
const aast = types.arrayPattern([types.identifier('a'), types.identifier('b')]);
const code = generate(aast);
fs.writeFileSync('./_babel/arrayPattern_file.js', code.code);

// 结果
[a, b]
```

5. **isArrayPattern**
```js
// 判断是不是“数组模式”
types.isArrayPattern(node)

// code
const array = [1,3,4];

const [a] = array;

console.log(a);

// use babel
const ast = parser.parse(code);

traverse(ast, {
    enter(path, state) {
        if(types.isArrayPattern(path)) {
            console.log(path.type, path.node);
        }
    }
})

// console
ArrayPattern Node {
  type: 'ArrayPattern',
  start: 30,
  end: 33,
  loc:
   SourceLocation {
     start: Position { line: 3, column: 6 },
     end: Position { line: 3, column: 9 },
     filename: undefined,
     identifierName: undefined },
  range: undefined,
  leadingComments: undefined,
  trailingComments: undefined,
  innerComments: undefined,
  extra: undefined,
  elements:
   [ Node {
       type: 'Identifier',
       start: 31,
       end: 32,
       loc: [SourceLocation],
       range: undefined,
       leadingComments: undefined,
       trailingComments: undefined,
       innerComments: undefined,
       extra: undefined,
       name: 'a' } ] }
```

6. **assertArrayPattern**
```
// 断言使用
types.assertArrayPattern(node)
```

## 字面量相关
1. **stringLiteral**
```js
// 创建string字面量
types.stringLiteral(value: string);

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.stringLiteral('test'))]);
const code = generage(ast);
console.log(code);

// console
var a = "test";
```

2. **bigIntLiteral**
```js
// 创建bigInt字面量：bigInt比number类型支持更大范围的整数值
types.bigIntLiteral(value: string);

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.stringLiteral('123'))]);
const code = generage(ast);
console.log(code);

// console
var a = 123n;
```

3. **booleanLiteral**
```js
// 创建boolean字面量
types.booleanLiteral(value: boolean);

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.booleanLiteral(true))]);
const code = generage(ast);
console.log(code);

// console
var a = true;
```

4. **nullLiteral**
```js
// 创建null字面量
types.nullLiteral();

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.nullLiteral())]);
const code = generage(ast);
console.log(code);

// console
var a = null;
```

5. **numberLiteral**
```js
// 创建number字面量
types.numberLiteral(value: number);

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.numberLiteral(123))]);
const code = generage(ast);
console.log(code);

// console
var a = 123;
```

6. **regExpLiteral**
```
// 创建regExp字面量
types.regExpLiteral(pattern: string, flags?: string);
// flags: g(全局模式)、i(忽略大小写)、m(多行模式)

// use babel
const ast = types.variableDeclaration('var', [types.variableDeclarator(types.identifier('a'), types.regExpLiteral('[0-9]'))]);
const code = generage(ast);
console.log(code);

// console
var a = /[0-9]/;
```
