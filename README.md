# action-stack-manager

manage redo/undo stack

管理撤销重做调用栈

1. always push (commit and rollback)
2. redo() ==> call commit
2. undo() ==> call rollback


## demo 

```
  const h = new HistoryManager()

  let inc = 0
  // 0 --> 2
  await h.push(() => inc = 2, () => inc = 0)
  // 2 --> 3
  await h.push(() => inc += 1, () => inc -= 1)

  console.log('HistoryManager', h, inc)
  console.log(this)
  // 3 --> 2
  await h.undo()
  console.log('undo', h.redoSize, h.undoSize, inc, 'inc = 2')
  // 2 --> 0
  await h.undo()
  console.log('undo', h.redoSize, h.undoSize, inc, 'inc = 0')
  // 0 --> 2
  await h.redo()
  console.log('redo', h.redoSize, h.undoSize, inc, 'inc = 2')
  // 2 --> 3
  await h.redo()
  console.log('redo', h.redoSize, h.undoSize, inc, 'inc = 3')
  // 报错
  await h.redo().catch(console.log)
  console.log('redo', h.redoSize, h.undoSize, inc, 'inc = 3')
  // 3 --> 2
  await h.undo()
  console.log('undo', h.redoSize, h.undoSize, inc, 'inc = 2')
  // 2 --> 4
  await h.push(() => inc += 2, () => inc -= 2)
  console.log('push', h.redoSize, h.undoSize, inc, 'inc = 4', h)
  // 4 --> 2
  await h.undo()
  console.log('undo', h.redoSize, h.undoSize, inc, 'inc = 2')
  // 2 --> 0
  await h.undo()
  console.log('undo', h.redoSize, h.undoSize, inc, 'inc = 0')
  // 0 --> 4
  await h.push(() => inc += 4, () => inc -= 4)
  console.log('push', h.redoSize, h.undoSize, inc, 'inc = 4', h)
```


