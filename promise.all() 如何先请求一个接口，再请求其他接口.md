```ts
     Promise.all([this.initDepartName(), this.initServiceName()]).then((res) => {
      console.log("res===>", res);
    });
  // 初始化部门接口
  async initDepartName() {
    return new Promise(() => {
      this.chartsService
        .postDimensionTree({ ...this.tiSelectDepartParams })
        .toPromise()
        .then((res) => {
          this.tiSelectDepart.options = res.data;
          this.tiSelectDepart.value = res.data.map((item) => item);
        });
    });
  }

  // 初始化服务接口
  async initServiceName() {
    return new Promise(() => {
      this.chartsService
        .postDimensionTree({ ...this.tiSelectServiceParams })
        .toPromise()
        .then((res) => {
          this.tiSelectService.options = res.data;
          this.tiSelectService.value = res.data.map((item) => item);
        });
    });
  }
```
