基于node.js/v8的中文开发环境

//从llvm中拿过来的StringSwitch
template<typename T, typename R = T>
class StringSwitch {
  std::wstring Str;
  const T *Result;

public:
  //
  explicit StringSwitch(std::wstring S)
  : Str(S), Result(nullptr) { }

  // StringSwitch is not copyable.
  StringSwitch(const StringSwitch &) = delete;
  void operator=(const StringSwitch &) = delete;

  StringSwitch(StringSwitch &&other) {
    *this = std::move(other);
  }
  StringSwitch &operator=(StringSwitch &&other) {
    Str = other.Str;
    Result = other.Result;
    return *this;
  }

  ~StringSwitch() = default;

  // Case-sensitive case matchers
  template<unsigned N>
  StringSwitch& Case(const wchar_t (&S)[N], const T& Value) {
    assert(N);
    if (!Result && N-1 == Str.size() &&
        //(N == 1 || std::memcmp(S, Str.data(), (N-1)*sizeof(wchar_t)) == 0)) {
        (N == 1 || Str.compare(S) == 0)) {
      Result = &Value;
    }
    return *this;
  }

  R Default(const T &Value) const {
    if (Result)
      return *Result;
    return Value;
  }

  
  operator R() const {
    assert(Result && "Fell off the end of a string-switch");
    return *Result;
  }
};
//中文的关键字对应
#define KEYWORDS(KEYWORD_GROUP, KEYWORD, ZWKEYWORD)                    \
  KEYWORD_GROUP('a')                                        \
  KEYWORD("arguments", Token::ARGUMENTS)                    \
  ZWKEYWORD("诸参", Token::ARGUMENTS)                    \
  KEYWORD("as", Token::AS)                                  \
  ZWKEYWORD("以", Token::AS)                                  \
  KEYWORD("async", Token::ASYNC)                            \
  ZWKEYWORD("异", Token::ASYNC)                            \
  KEYWORD("await", Token::AWAIT)                            \
  ZWKEYWORD("等", Token::AWAIT)                            \
  KEYWORD("anonymous", Token::ANONYMOUS)                    \
  ZWKEYWORD("匿", Token::ANONYMOUS)                    \
  KEYWORD_GROUP('b')                                        \
  KEYWORD("break", Token::BREAK)                            \
  ZWKEYWORD("破", Token::BREAK)                            \
  KEYWORD_GROUP('c')                                        \
  KEYWORD("case", Token::CASE)                              \
  ZWKEYWORD("例", Token::CASE)                              \
  KEYWORD("catch", Token::CATCH)                            \
  ZWKEYWORD("捕", Token::CATCH)                            \
  KEYWORD("class", Token::CLASS)                            \
  ZWKEYWORD("类", Token::CLASS)                            \
  KEYWORD("const", Token::CONST)                            \
  ZWKEYWORD("常", Token::CONST)                            \
  KEYWORD("constructor", Token::CONSTRUCTOR)                \
  ZWKEYWORD("构造器", Token::CONSTRUCTOR)                \
  KEYWORD("continue", Token::CONTINUE)                      \
  ZWKEYWORD("继", Token::CONTINUE)                      \
  KEYWORD_GROUP('d')                                        \
  KEYWORD("debugger", Token::DEBUGGER)                      \
  ZWKEYWORD("调试器", Token::DEBUGGER)                      \
  KEYWORD("default", Token::DEFAULT)                        \
  ZWKEYWORD("默", Token::DEFAULT)                        \
  KEYWORD("delete", Token::DELETE)                          \
  ZWKEYWORD("删", Token::DELETE)                          \
  KEYWORD("do", Token::DO)                                  \
  ZWKEYWORD("行", Token::DO)                                  \
  KEYWORD_GROUP('e')                                        \
  KEYWORD("else", Token::ELSE)                              \
  ZWKEYWORD("另", Token::ELSE)                              \
  KEYWORD("enum", Token::ENUM)                              \
  ZWKEYWORD("举", Token::ENUM)                              \
  KEYWORD("eval", Token::EVAL)                              \
  ZWKEYWORD("估", Token::EVAL)                              \
  KEYWORD("export", Token::EXPORT)                          \
  ZWKEYWORD("导", Token::EXPORT)                          \
  KEYWORD("extends", Token::EXTENDS)                        \
  ZWKEYWORD("承", Token::EXTENDS)                        \
  KEYWORD_GROUP('f')                                        \
  KEYWORD("false", Token::FALSE_LITERAL)                    \
  ZWKEYWORD("假", Token::FALSE_LITERAL)                    \
  KEYWORD("finally", Token::FINALLY)                        \
  ZWKEYWORD("末", Token::FINALLY)                        \
  KEYWORD("for", Token::FOR)                                \
  ZWKEYWORD("于", Token::FOR)                                \
  KEYWORD("from", Token::FROM)                              \
  ZWKEYWORD("从", Token::FROM)                              \
  KEYWORD("function", Token::FUNCTION)                      \
  ZWKEYWORD("函数", Token::FUNCTION)                      \
  KEYWORD_GROUP('g')                                        \
  KEYWORD("get", Token::GET)                                \
  ZWKEYWORD("取", Token::GET)                                \
  KEYWORD_GROUP('i')                                        \
  KEYWORD("if", Token::IF)                                  \
  ZWKEYWORD("如", Token::IF)                                  \
  KEYWORD("implements", Token::FUTURE_STRICT_RESERVED_WORD) \
  ZWKEYWORD("成", Token::FUTURE_STRICT_RESERVED_WORD) \
  KEYWORD("import", Token::IMPORT)                          \
  ZWKEYWORD("引", Token::IMPORT)                          \
  KEYWORD("in", Token::IN)                                  \
  ZWKEYWORD("在", Token::IN)                                  \
  KEYWORD("instanceof", Token::INSTANCEOF)                  \
  ZWKEYWORD("是为", Token::INSTANCEOF)                  \
  KEYWORD("interface", Token::FUTURE_STRICT_RESERVED_WORD)  \
  ZWKEYWORD("接", Token::FUTURE_STRICT_RESERVED_WORD)  \
  KEYWORD_GROUP('l')                                        \
  KEYWORD("let", Token::LET)                                \
  ZWKEYWORD("让", Token::LET)                                \
  KEYWORD_GROUP('m')                                        \
  KEYWORD("meta", Token::META)                              \
  ZWKEYWORD("元", Token::META)                                \
  KEYWORD_GROUP('n')                                        \
  KEYWORD("name", Token::NAME)                              \
  ZWKEYWORD("名", Token::NAME)                              \
  KEYWORD("new", Token::NEW)                                \
  ZWKEYWORD("新", Token::NEW)                                \
  KEYWORD("null", Token::NULL_LITERAL)                      \
  ZWKEYWORD("无", Token::NULL_LITERAL)                      \
  KEYWORD_GROUP('o')                                        \
  KEYWORD("of", Token::OF)                                  \
  ZWKEYWORD("之", Token::OF)                                  \
  KEYWORD_GROUP('p')                                        \
  KEYWORD("package", Token::FUTURE_STRICT_RESERVED_WORD)    \
  ZWKEYWORD("包", Token::FUTURE_STRICT_RESERVED_WORD)    \
  KEYWORD("private", Token::FUTURE_STRICT_RESERVED_WORD)    \
  ZWKEYWORD("私", Token::FUTURE_STRICT_RESERVED_WORD)    \
  KEYWORD("protected", Token::FUTURE_STRICT_RESERVED_WORD)  \
  ZWKEYWORD("保", Token::FUTURE_STRICT_RESERVED_WORD)  \
  KEYWORD("prototype", Token::PROTOTYPE)                    \
  ZWKEYWORD("原型", Token::PROTOTYPE)                    \
  KEYWORD("public", Token::FUTURE_STRICT_RESERVED_WORD)     \
  ZWKEYWORD("公", Token::FUTURE_STRICT_RESERVED_WORD)     \
  KEYWORD_GROUP('r')                                        \
  KEYWORD("return", Token::RETURN)                          \
  ZWKEYWORD("返", Token::RETURN)                          \
  KEYWORD_GROUP('s')                                        \
  KEYWORD("set", Token::SET)                                \
  ZWKEYWORD("设", Token::SET)                                \
  KEYWORD("static", Token::STATIC)                          \
  ZWKEYWORD("固", Token::STATIC)                          \
  KEYWORD("super", Token::SUPER)                            \
  ZWKEYWORD("超", Token::SUPER)                            \
  KEYWORD("switch", Token::SWITCH)                          \
  ZWKEYWORD("切", Token::SWITCH)                          \
  KEYWORD_GROUP('t')                                        \
  KEYWORD("target", Token::TARGET)                          \
  ZWKEYWORD("标", Token::TARGET)                          \
  KEYWORD("this", Token::THIS)                              \
  ZWKEYWORD("此", Token::THIS)                              \
  KEYWORD("throw", Token::THROW)                            \
  ZWKEYWORD("抛", Token::THROW)                            \
  KEYWORD("true", Token::TRUE_LITERAL)                      \
  ZWKEYWORD("真", Token::TRUE_LITERAL)                      \
  KEYWORD("try", Token::TRY)                                \
  ZWKEYWORD("试", Token::TRY)                                \
  KEYWORD("typeof", Token::TYPEOF)                          \
  ZWKEYWORD("之型", Token::TYPEOF)                          \
  KEYWORD_GROUP('u')                                        \
  KEYWORD("undefined", Token::UNDEFINED)                    \
  ZWKEYWORD("未定", Token::UNDEFINED)                    \
  KEYWORD_GROUP('v')                                        \
  KEYWORD("var", Token::VAR)                                \
  ZWKEYWORD("变", Token::VAR)                                \
  KEYWORD("void", Token::VOID)                              \
  ZWKEYWORD("空", Token::VOID)                              \
  KEYWORD_GROUP('w')                                        \
  KEYWORD("while", Token::WHILE)                            \
  ZWKEYWORD("当", Token::WHILE)                            \
  KEYWORD("with", Token::WITH)                              \
  ZWKEYWORD("为之", Token::WITH)                              \
  KEYWORD_GROUP('y')                                        \
  KEYWORD("yield", Token::YIELD)                            \
  ZWKEYWORD("降", Token::YIELD)                            \
  KEYWORD_GROUP('_')                                        \
  KEYWORD("__proto__", Token::PROTO_UNDERSCORED)            \
  ZWKEYWORD("__原__", Token::PROTO_UNDERSCORED)              \
  KEYWORD_GROUP('#')                                        \
  KEYWORD("#constructor", Token::PRIVATE_CONSTRUCTOR)        \
  ZWKEYWORD("#构造器", Token::PRIVATE_CONSTRUCTOR)

//内置对象和函数修改实例：
 添加 “控制台.日志” 等效 “console.log”
 
lib\internal\bootstrap\node.js // v10
lib\internal\bootstrap_node.js // v8
// 仿照添加 “控制台” 等效 “console”
    Object.defineProperty(global, 'console', {
      configurable: true,
      enumerable: true,
      get() {
        return wrappedConsole;
        }
    });
    Object.defineProperty(global, '控制台', {
      configurable: true,
      enumerable: true,
      get() {
        return wrappedConsole;
       }
    });


// lib\console.js
// 添加一条“日志” 等效 “log”
Console.prototype.debug = Console.prototype.log;

Console.prototype.日志 = Console.prototype.log;

Console.prototype.info = Console.prototype.log;

运行实例：
hello.js
函数 说1(词) {
    控制台.日志(词);
}
函数 执行(某函数, 值) {
    某函数(值);
}
变 你 = "你"
变 哈哈 = {
    哈1: '好',
     哈2: 12
}

执行(说1, 你);
控制台.日志(哈哈);

运行结果:
./node hello.js
你
{ '哈1': '好', '哈2': 12 }
