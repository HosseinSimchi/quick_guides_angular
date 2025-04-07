### Components

- If you have:

```javascript
customMethod(data: any){
data.collapse();
data.expand();

  return this.getDataByService(node);
}
```

- Test corresponding to the above method

```javascript
it("#customMethod - should call getDataByService()", () => {
  spyOn(component, "getDataByService");

  const data = {
    collapse: jasmine.createSpy("collapse"),
    expand: jasmine.createSpy("collapse"),
  };

  component.customMethod(data);

  expect(data.collapse).toHaveBeenCalled();
  expect(data.expand).toHaveBeenCalled();
  expect(component.getDataByService).toHaveBeenCalled();
});
```
