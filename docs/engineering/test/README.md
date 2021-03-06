在软件测试中，测试自动化（英语：Test automation）是一种测试方法，使用特定的软件，去控制测试流程，并比较**实际的结果与预期结果**之间的差异。通过将测试自动化，可以让正式的测试过程中的必要测试，可以反复进行；通过这种方法，也可以将难以手动进行的测试，交由软件来做。

总结一下：自动化测试指软件测试的自动化，让软件代替人工测试，可以快速、反复进行。

## 前端自动化测试分类

前端自动化测试整体大致分为以下四类：单元测试、集成测试、ui 测试、端到端(e2e)测试/功能测试，虽各有场景，但也在“不同阶段“各有优劣。这里必须强调是“不同阶段”，才会产生不同的选择优劣，因为每一种自动化测试本身，都有其实际不可替代的场景。

### 单元测试

是指对应用中的最小可测试单元进行检查和验证，通常指的是独立测试单个函数。

前端单元测试，是所有类型的前端自动化测试中成本较低，且 90%以上前端逻辑场景都可覆盖的，非常有效的自动化测试手段。

**一个简单的测试用例**

```js
function add(a, b) {
  return a + b
}

function expect(result) {
  return {
    toBe: function(actual) {
      if (result === actual) {
        console.log('test successfully!')
      } else {
        throw new Error(`expect ${actual}, but return ${result}`)
      }
    },
  }
}

// 1 + 2 = 3
expect(add(1, 2)).toBe(3)
// 0.1 + 0.2 = 0.3
expect(add(0.1, 0.2)).toBe(0.3)
```

### 集成测试

就是测试应用中完全不同的模块(组件)间，如何集成、如何一起工作。这和它的名字一致，用来确保不同组件间互相合作的正确性。

### UI 测试

是对图形交互界面的测试。UI 层是用户使用产品的入口，所有功能通过这一层提供给用户。

### 端到端(e2e)测试

是站在用户角度的测试，把我们的程序看成是一个黑盒子，用来确保整个应用会按照用户期望的那样运行。
