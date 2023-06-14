# 请求：

```js
api.update(this.sysRole)
    .then(() => {  //成功
       // 提示
    this.$message({ type: "success", message: "修改成功" });
    this.fetchData(); // 刷新表格
})
    .catch(()=>{ // 失败
       // 提示
    this.$message({ type: "error", message: "数据异常，请稍后重试!" 
});
```

