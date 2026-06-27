---
名称: EBNF 转递归下降解析器
章: 01 编译原理
节: "[[本科/大二下/01 编译原理/04 自顶向下分析/第四章 自顶向下分析]]"
tags:
  - 知识点
index: 1
---
将 **EBNF（Extended Backus-Naur Form）** 文法直接转换为递归下降解析器的代码EBNF 使用花括号 `{...}` 表示重复，方括号 `[...]` 表示可选结构

##### 条件语句的转换

**EBNF 规则**：
```ebnf
if_stmt → if ( exp ) statement [ else statement ]
```

**代码实现**：
```pascal
procedure parse_if_stmt;
begin
    match(if);
    match(();
    parse_exp;
    match());
    parse_statement;
    if current_token = else then
        match("else");
        parse_statement;
    end if;
end;
```

- **可选分支处理**：通过 `if current_token = else` 检测可选分支
- **前瞻匹配**：`match` 函数验证当前 token 并消费

##### 表达式与项的转换

**EBNF 规则**：
```ebnf
exp   → term { addop term }    // addop = + | -
term  → factor { mulop factor } // mulop = *
```

**伪代码实现**：
```pascal
procedure parse_exp;
var temp: integer;
begin
    temp := parse_term;
    while current_token in ["+", "-"] do
        case current_token of
            "+":
                match("+");
                temp := temp + parse_term;
            "-":
                match("-");
                temp := temp - parse_term;
        end case;
    end while;
    return temp;
end;

procedure parse_term;
var temp: integer;
begin
    temp := parse_factor;
    while current_token = "*" do
        match("*");
        temp := temp * parse_factor;
    end while;
    return temp;
end;
```

> [!note]

##### 计算器实现（C 代码）

```c
#include <stdio.h>
#include <stdlib.h>

char token;

void match(char expected) {
    if (token == expected) {
        token = getchar();
    } else {
        fprintf(stderr, "Syntax error: expected '%c'\n", expected);
        exit(1);
    }
}

int parse_factor() {
    int value;
    if (token >= '0' && token <= '9') {
        value = token - '0';
        match(token);
    } else {
        fprintf(stderr, "Unexpected token: '%c'\n", token);
        exit(1);
    }
    return value;
}

int parse_term() {
    int value = parse_factor();
    while (token == '*') {
        match('*');
        value *= parse_factor();
    }
    return value;
}

int parse_exp() {
    int value = parse_term();
    while (token == '+' || token == '-') {
        if (token == '+') {
            match('+');
            value += parse_term();
        } else {
            match('-');
            value -= parse_term();
        }
    }
    return value;
}

int main() {
    token = getchar();
    int result = parse_exp();
    if (token == '\n') {
        printf("Result: %d\n", result);
    } else {
        fprintf(stderr, "Unexpected token: '%c'\n", token);
        return 1;
    }
    return 0;
}
```

##### 语法树构建示例

在解析过程中构建语法树，而非直接求值：

```pascal
function parse_exp: SyntaxTree;
var
    left, right: SyntaxTree;
    op: Operator;
begin
    left := parse_term;
    while current_token in ["+", "-"] do
        op := parse_addop;
        right := parse_term;
        left := create_node(op, left, right);
    end while;
    return left;
end;
```

##### 关键挑战

1. **BNF 到 EBNF 的转换困难**：需处理递归结构和重复/可选符号的引入，错误转换可能导致结构歧义或循环解析
2. **产生式选择的歧义性**：相同前缀的产生式需通过 First/Follow 集合解决
