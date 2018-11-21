# fastest-validator-using
fastest-validator使用
一、安装
npm: npm install fastest-validator --save
yarn: yarn add fastest-validator

二、基本流程：
	1.配置校验规则
	2.根据配置的规则验证json参数
	3.支持13种类型校验规则配置
	4.支持两种验证函数，一种普通函数，一种快速函数，快速函数比普通函数快10倍左右

三、一个简单的例子
```javascript
// 引入快速校验包
let Validator = require("fastest-validator");
// 初始化
let v = new Validatro();
// 配置校验规则
const schema = {
	// 需要校验的字段名，json key值 严格相等
	id: {
		// 字段类型 number 共支持13种类型
		type: "number",
		// number类型参数 是否是正数							
		positive: true,
		// number类型参数 是否是整数
		integer: true							
	}，
	// 需要校验的字段名，json key值 严格相等
	name: {
		// 字段类型 string 共支持13种类型
		type: "string",
		// string类型参数 字符串最小长度
		min: 3,
		// string类型参数 字符串最大长度
		max: 255
	}
};
// 普通方法
v.validate({id: 5, name: "John"}, schema);
// 快速方法 比普通方法快10倍左右
let check = v.compile(schema)；
check({id: 5, name: "John"});
```

三、支持校验的类型
type参数支持13种类型
1.any
任何类型

2.array
数组

<div class="bi-table">
  <table>
    <colgroup>
      <col width="86px" />
      <col width="90px" />
      <col width="509px" />
    </colgroup>
    <tbody>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">参数</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">类型</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">描述</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">empty</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">boolean</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">是否能为空</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">min</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">number</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">最小长度</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">max</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">number</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">最大长度</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">length</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">number</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">固定长度</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">iteams</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">object</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">指定数组元素类型可以使任何类型，见下方示例</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">contains</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">string</div>
          <div data-type="p">number</div>
          <div data-type="p">boolean</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">必须包含的内容，测试只支持 string、number、boolean类型，不支持array object，见下方示例</div>
        </td>
      </tr>
      <tr height="34px">
        <td rowspan="1" colSpan="1">
          <div data-type="p">enum</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">array</div>
        </td>
        <td rowspan="1" colSpan="1">
          <div data-type="p">数组元素只能包含这个数组里的元素，见下方示例</div>
        </td>
      </tr>
    </tbody>
  </table>
</div>

iteams参数示例：
```javascript
// 校验规则
const arrayIteamsSchema = {
	arrayIteams: {
		type: "array",
		items: {
           // 数组元素为object
			type: "object",
			props: {
               // 数组元素object的第1个参数为number
				id: {
					type: "number"
				},
               // 数组元素object的第2个参数为string
				name: {
					type: "string"
				},
               // 数组元素object的第3个参数为object
				classInfo: {
					type: "object",
					props: {
                      // number
						classId: {
							type: "number"
						},
                      // string
						className: {
							type: "string"
						},
                      // array
						teacher: {
							type: "array"
							items: {
								type: "object",
								props: {
									teacherId: {
										type: "number"
									},
									teacherName: {
										type: "string"
									}
								}
							}
						}
					}
				}
			}
		}
	}
};
// 能通过校验的数据
let testArray = {
	arrayTest: [
		{
			id: 1,
			name: "zilla1",
			class: {
				classId: 1,
				className: "calss1",
				teacher: [
					{
						teacherId: 1,
						teacherName: "wang1"
					},
					{
						teacherId: 2,
						teacherName: "wang2"
					}
				]
			}
		},
		{
			id: 2,
			name: "zilla2",
			class: {
				classId: 2,
				className: "calss2",
				teacher: [
					{
						teacherId: 1,
						teacherName: "wang1"
					},
					{
						teacherId: 2,
						teacherName: "wang2"
					},
					{
						teacherId: 3,
						teacherName: "wang3"
					}
				]
			}
		},
		{
			id: 3,
			name: "zilla3",
			class: {
				classId: 3,
				className: "calss3",
				teacher: [
					{
						teacherId: 1,
						teacherName: "wang1"
					},
					{
						teacherId: 2,
						teacherName: "wang2"
					}
				]
			}
		}
	]
};
// 能通过校验
v.validate(testArray, arrayIteamsSchema);
```

contains参数示例：在arrayContians数组中必须包含true才能通过校验
```javascript
// 校验规则
const arrayContainsSchema = {
	arrayContains: {
		type: "array",
		items: {
			type: "boolean"
		},
		contains: true
	}
};
// 能通过校验
v.validate(
    {arrayContains: [true, false]},
    arrayContainsSchema
);
```

enum参数示例：在arrayEnum数组中只能包含enum数组里的元素才能通过校验
```javascript
// 校验规则
const arrayEnumSchema = {
	arrayEnum: {
		type: "array",
		min: 1,
		max: 5,
		items: "string",
		enum: ["test1", "test2"]
	}
};
// 能通过校验
v.validate(
    {arrayEnum: ["test1", "test2"]},
    arrayEnumSchema
);
```

3.boolean
布尔值

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| convert | boolean | 转变 0 "0" "off" "false" 1 "1" "on" "true" 为一个boolean值，前4个转为false，后4个转为true |

示例：
```javascript
// 校验规则
const booleanSchema = {
    booleanTest: {
        type: "boolean",
        convert: true
    }
};
// 能通过校验
v.validate(
    {booleanTest: "off"},
    booleanSchema
);
```

4.date
日期

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| convert | boolean | 不是Date类型，尝试转换为Date类型 |

示例：
```javascript
// 校验规则
const dateSchema = {
	dateTest: {
		type: "date",
		convert: true
	}
};
// 能通过校验
v.validate(
    {dateTest: new Date()}，
    dateSchema
);
// 能通过校验
v.validate(
    {dateTest: 1488876927958},
    dateSchema
);
// 不能通过校验
v.validate(
    {dateTest: "1488876927958"},
    dateSchema
);
```

5.email
邮箱

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| mode | string | quick/precies 快速和精细两个模式，精细模式用了更为复杂的正则表达式，快速模式的正则表达式较简单 |

示例：
```javascript
// 校验规则
const emailSchema = {
	emailTest: {
		type: "email",
		mode: "precise"
	}
};
// 能通过校验
v.validate(
    {emailTest: "test@gmail.com"},
    emailSchema
);
```

6.enum
枚举

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| values | array | 待校验的值只能是该数组里的一个值，只支持简单类型string、number、boolean |

示例：
```javascript
// 校验规则
const enumSchema = {
	enumTest: {
		type: "enum",
		values: ["test1", "test2"]
	}
};
// 能通过校验
v.validate(
    {enumTest: "test1"},
    enumSchema
);
// 不能通过校验
v.validate(
    {enumTest: "test3"},
    enumSchema
);
```

7.forbidden
不能有该属性
示例：
```javascript
// 校验规则
const forbiddenSchema = {
	userName: {
		type: "string"
	},
	password: {
		type: "forbidden"
	}
};
// 不能通过校验
v.validate(
    {
        userName: "admin",
	    password: "123456"
    },
    forbiddenSchema
);
// 能通过校验
v.validate(
    {userName: "admin"},
    forbiddenSchema
);
```

8.function
函数
示例：
```javascript
// 校验规则
const functionSchema = {
	functionTest: {
		type: "function"
	}
};

let fnc= function() {};
// 能通过校验
v.validate({functionTest: () => {}}, functionSchema);
// 能通过校验
v.validate({functionTest: function() {}}, functionSchema);
// 能通过校验
v.validate({functionTest: fnc}, functionSchema);
// 能通过校验
v.validate({functionTest: Date.now}, functionSchema);
```

9.number
数字

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| min | number | 最小值 |
| max | number | 最大值 |
| equal | number | 等于这个值 |
| notEqual | number | 不等于这个值 |
| integer | ​boolean | 是否是整数 |
| positive | boolean | 是否是正数 |
| negative | boolean | 是否是负数 |
| convert | boolean | 不是数字，使用parseFloat函数转为浮点型，如果不能转返回NaN |

convert示例：
```javascript
// 校验规则
const numberConvertSchema = {
	numberConvertTest: {
		type: "number",
		convert: true
	}
};
// 不能通过校验
v.validate({numberConvertTest: "abc"},numberConvertSchema);
// 能通过校验
v.validate({numberConvertTest: "123"},numberConvertSchema)
```

10.object
对象

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| props | object | 对这个对象字段的内容进行规则编写，同为13种类型 |

示例：
```javascript
// 校验规则
const objectSchema = {
	objectTest: {
		type: "object",
		props :{
			name: {
				type: "string",
				min: 3,
				max: 18
			},
			age: {
				type: "number",
				positive: true,
				integer: true
			}
		}
	}
};
// 能通过校验
v.validate(
	{
		objectTest: {
			name: "test1",
			age: 18
		}
	}
    , objectSchema);
```

11.string
字符串

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| empty | boolean | 是否能为空字符串 |
| min | number | 最小长度 |
| max | number | 最大长度 |
| length | number | 固定长度 |
| pattern | string | 正则 |
| contains | ​string | 包含部分字符串 |
| enum | array | 字符串是否是枚举数组里的变量 |
| alpha | boolean | 是否必须是字母字符串 |
| numeric | boolean | 是否必须是数字字符串 |
| alphanum | boolean | 是否必须是一个字母数字字符串 |
| alphadash | boolean | 是否必须是包含破折号的字母字符串 |

pattern参数示例：
```javascript
// 校验规则
const stringSchema = {
	stringTest: {
		type: "string",
		pattern: /^https?:\/\/\S+/
	}
};
// 能通过校验
v.validate(
	{stringTest: "http://www.baidu.com"},
	stringSchema
);

```

contains参数示例：
```javascript
// 校验规则
const stringSchema = {
	stringTest: {
		type: "string",
		contains: "abc",
	}
}
// 能通过校验
v.validate(
	{stringTest: "abcd"},
	stringSchema
);
// 不能通过校验
v.validate(
	{stringTest: "bcde"},
	stringSchema
);
```

enum参数示例：
```javascript
// 校验规则
const stringSchema = {
	stringTest: {
		type: "string",
		enum: ["abc", "bcd", "cde"]
	}
};
// 能通过校验
v.validate(
	{stringTest: "abc"},
	stringSchema
);
// 不能通过校验
v.validate(
	{stringTest: "abcd"},
	stringSchema
);
```


12.url
url字符串，类似邮箱用正则表达式校验

13.custom
自定义
需要在初始化fastest-validator组件时，添加自定义的错误信息
方法1：
```javascript
let v = new Validator({
	messages: {
		// 注册多个新的错误信息，错误类型为evenNumber, evenMin, evenMax对应规则配置的type,evenMin和evenMax
		evenNumber: "The '{field}' field must be an even number! Actual: {actual}",
		evenMin: "The '{field}' field must bigger than {expected}! Actual: {actual}",
		evenMax: "The '{field}' field must smaller than {expected}! Actual: {actual}"
	}
});
// 注册新的校验逻辑，如果是校验失败通过v.makeError方法返回错误信息，v.makeError方法第一个参数为错类型，第二个参数为期望值，第三个参数为实际值
v.add("even", (value, schema) => {
	if(typeof value !== "number")
		return v.makeError("evenNumber", null, value);
	if(value < schema.evenMin)
		return v.makeError("evenMin", schema.evenMin, value);
	if(value > schema.evenMax)
		return v.makeError("evenMax", schema.evenMax, value);
	return true;
});
// 编写校验规则
const schema = {
	name: { type: "string", min: 3, max: 255 },
	age: { type: "even" , evenMin: 8, evenMax: 18}
};
// 测试
console.log(v.validate({ name: "John", age: "abc" }, schema));// 校验不通过，age不是evenNumber
console.log(v.validate({ name: "John", age: 19 }, schema));	// 校验不通过，age比18大
console.log(v.validate({ name: "John", age: 6 }, schema));		// 校验不通过，age比8小
console.log(v.validate({ name: "John", age: 18 }, schema));	// 校验通过
```
方法2：
```javascript
let v = new Validator({
	messages: {
        // 注册多个新的错误信息，错误类型为evenNumber, evenMin, evenMax对应规则配置的type,evenMin和evenMax
		evenNumber: "The '{field}' field must be an even number! Actual: {actual}",
		evenMin: "The '{field}' field must bigger than {expected}! Actual: {actual}",
		evenMax: "The '{field}' field must smaller than {expected}! Actual: {actual}"
	}
});
// 编写校验规则，加入check函数注册新的校验逻辑
const schema = {
    name: { type: "string", min: 3, max: 255 },
    age: {
        type: "custom",
        minEven: 8,
        maxEven: 18,
        // 校验逻辑
        check(value, schema) {
            if(typeof value !== "number")
                return v.makeError("evenNumber", null, value);
            if(value < schema.minEven)
                return v.makeError("evenMin", schema.minEven, value);
            if(value > schema.maxEven)
                return v.makeError("evenMax", schema.maxEven, value);
            return true;
        }
    }
};
```
在初始化fastest-validator包的同时，除了能注册新的错误信息，能修改默认错误信息，如将stringMin和stringMax的错误描述信息修改成中文
```javascript
const Validator = require("fastest-validator");
const v = new Validator({
    messages: {
        // 默认的stringMin和strinMax信息
        // stringMin: "The '{field}' field length must be greater than or equal to {expected} characters long!",
        // stringMax: "The '{field}' field length must be less than or equal to {expected} characters long!",
        stringMin: "字段'{field}'的长度必须大于等于{expected}，实际长度为: {actual}",
        stringMax: "字段'{field}'的长度必须小于等于{expected}，实际长度为: {actual}"
    }
});

```
四、校验结果
1.正确结果返回true
2.错误结果是对象数组，每一个错误都是一个数组元素
```plain
[	
    {
		type:		错误类型
		expected:	期望值
		actual:		实际值
		field:		字段
		message：	错误信息
	}
]
```

3.错误类型

| 错误名 | 错误信息 |
| :--- | :--- |
| required | The '{field}' field is required! |
| string | The '{field}' field must be a string! |
| stringEmpty | The '{field}' field must not be empty! |
| stringMin | The '{field}' field length must be greater than or equal to {expected} characters long! |
| stringMax | The '{field}' field length must be less than or equal to {expected} characters long! |
| stringLength | The '{field}' field length must be {expected} characters long! |
| stringPattern | The '{field}' field fails to match the required pattern! |
| stringContains | The '{field}' field must contain the '{expected}' text! |
| stringEnum | The '{field}' field does not match any of the allowed values! |
| number | The '{field}' field must be a number! |
| numberMin | The '{field}' field must be greater than or equal to {expected}! |
| numberMax | The '{field}' field must be less than or equal to {expected}! |
| numberEqual | The '{field}' field must be equal with {expected}! |
| numberNotEqual | The '{field}' field can't be equal with {expected}! |
| numberInteger | The '{field}' field must be an integer! |
| numberPositive | The '{field}' field must be a positive number! |
| numberNegative | The '{field}' field must be a negative number! |
| array | The '{field}' field must be an array! |
| arrayEmpty | The '{field}' field must not be an empty array! |
| arrayMin | The '{field}' field must contain at least {expected} items! |
| arrayMax | The '{field}' field must contain less than or equal to {expected} items! |
| arrayLength | The '{field}' field must contain {expected} items! |
| arrayContains | The '{field}' field must contain the '{expected}' item! |
| arrayEnum | The '{field} field value '{expected}' does not match any of the allowed values! |
| boolean | The '{field}' field must be a boolean! |
| function | The '{field}' field must be a function! |
| date | The '{field}' field must be a Date! |
| dateMin | The '{field}' field must be greater than or equal to {expected}! |
| dateMax | The '{field}' field must be less than or equal to {expected}! |
| forbidden | The '{field}' field is forbidden! |
| email | The '{field}' field must be a valid e-mail! |

五、其它
1.普通方法每次验证都需要编译一次验证规则，而快速方法将验证规则变异成check函数，需要验证数据时直接调用check函数就行。当一个规则验证次数少的时候可以用普通方法，需要多次用到一个验证规则时建议使用快速方法
2.支持一个参数多个规则，多个规则以数组的形式表达
示例：
```javascript
const schema = {
	multi: [
		{ type: "string", min: 3, max: 255 },
		{ type: "boolean" }
	]
};
// 能通过校验
v.validate({ multi: "John" }, schema);
// 能通过校验
v.validate({ multi: true }, schema);
// 不能通过校验
v.validate({ multi: "Al" }, schema);

```

3.规则中每个字段都是必须的，如果要将字段设置为可选的，需要配置参数 optional: true
示例：
```javascript
const schema = {
    name: { type: "string" }, // required
    age: { type: "number", optional: true }
}
// 能通过校验
v.validate({ name: "John", age: 42 }, schema);
// 能通过校验
v.validate({ name: "John" }, schema);
// 不能通过校验
v.validate({ age: 42 }, schema);
```

