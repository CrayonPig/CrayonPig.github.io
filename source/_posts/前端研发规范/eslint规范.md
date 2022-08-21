


```javascript
 {
    // 要求使用扩展运算符而非 .apply()
    "prefer-spread": "off",
    '@typescript-eslint/no-var-requires': 0,
    // 禁止出现未使用过的变量
    "no-unused-vars": "off",
    "@typescript-eslint/no-unused-vars": "off",
    // 关闭要求使用 const 声明那些声明后不再被修改的变量
    "prefer-const": 'off',
    '@typescript-eslint/no-non-null-assertion': 'off',
    "@typescript-eslint/no-explicit-any": ["off"],
    // 关闭强制使用骆驼拼写法命名约定
    "@typescript-eslint/camelcase": ["off"],
    // 禁用 tab
    'no-tabs': 0,
    // 禁止空格和 tab 的混合缩进
    'no-mixed-spaces-and-tabs': 0,
    // 禁用行尾空格
    'no-trailing-spaces': 0,
    // 强制使用一致的缩进
    "indent": [
      "error",
      2,
      { "SwitchCase": 1 }
    ],
    // 强制使用一致的反勾号、双引号或单引号
    "quotes": [ "error", "single" ],
    // 要求或禁止使用分号代替 ASI
    "semi": [ "error", "always" ],
    // 在逗号后需要一个或多个空格
    "comma-spacing": [
      "error",
      {
        "after": true
      }
    ],
    // 禁止或强制在代码块中开括号前和闭括号后有空格
    "block-spacing": [
      "error",
      "never"
    ],
    // 强制在代码块中使用一致的大括号风格
    "brace-style": [
      "error",
      "1tbs"
    ],
    // 强制在关键字前后使用一致的空格
    "keyword-spacing": [
      "error",
      {
        "before": true,
        "after": true
      }
    ],
    // 强制在对象字面量的属性中键和值之间使用一致的间距
    "key-spacing": [
      "error",
      {
        "beforeColon": false,
        "afterColon": true
      }
    ],
    // 禁用行尾空格
    "no-trailing-spaces": "error",
    // 强制在大括号中使用一致的空格
    "object-curly-spacing": [
      "error",
      "always"
    ],
    // 要求操作符周围有空格
    "space-infix-ops": ["error", { "int32Hint": true }],
    // 强制在 function的左括号之前使用一致的空格
    "space-before-function-paren": [
      'error',
      "always"
    ],
    // 强制在圆括号内使用一致的空格
    "space-in-parens": [
      "error",
      "never"
    ],
    // 强制在块之前使用一致的空格
    "space-before-blocks": [
      "error",
      "always"
    ],
    // 关闭禁用 console
    "no-console": "off",
    // 关闭禁止在 return、throw、continue 和 break 语句之后出现不可达代码
    "no-unreachable": "off",
    // 禁止在变量定义之前使用它们
    "no-use-before-define": ["error", { "functions": false, "classes": false }],
    // 禁止在正则表达式中使用控制字符
    "no-control-regex": "off",
    // 关闭禁用 debugger
    "no-debugger": "off",
    // 禁止 case 语句落空
    "no-fallthrough": ["error", { "commentPattern": "break[\\s\\w]*omitted" }],
    // 不需要强制使用全等
    "eqeqeq": "off"
  }
```


