# leetcode刷题 c++版本

## 数据结构相关

## 1. 栈和队列

#### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)（简单）

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列的支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

  ```c++
  class MyQueue {
  private:
      stack<int> instack, outstack;
      void in2out(){
          while(!instack.empty()){
              outstack.push(instack.top());
              instack.pop();
          }
      }
      
  public:
      /** Initialize your data structure here. */
      MyQueue() {
      }
      
      /** Push element x to the back of queue. */
      void push(int x) {
          instack.push(x);
      }
      
      /** Removes the element from in front of queue and returns that element. */
      int pop() {
          if(outstack.empty()){
              in2out();
          }
          int x = outstack.top();
          outstack.pop();
          return x;
      }
      
      /** Get the front element. */
      int peek() {
          if(outstack.empty()){
              in2out();
          }
          return outstack.top();
      }
      
      /** Returns whether the queue is empty. */
      bool empty() {
          return instack.empty() && outstack.empty();
      }
  };   
  ```

## 2. 算法思想

### 2.1 双指针

### 2.2 排序

