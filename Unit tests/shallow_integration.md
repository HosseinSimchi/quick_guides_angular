**1- Should call customMethod() when user clicks on the tag**

- ```html
  <span (click)="closeModal()"> </span>
  ```

- ```javascript
  it("#shallow UT - should call customMethod() when user clicks on the <span />", () => {
    spyOn(component, "customMethod");

    const tag = el.query(By.css("span"));
    tag.nativeElement.click();

    expect(component.customMethod).toHaveBeenCalled();
  });
  ```

**2- Should render the tag if the customMethod returns true**

- ```javascript
  @if(customMethod()){
  <span></span>
  }
  ```

- ```javascript
  it("#shallow UT - should render <span /> if customMethod() returns true", () => {
    spyOn(component, "customMethod").and.reutrnValue(true);
    fixture.detectChanges();

    const tag = el.query(By.css("span"));

    expect(tag).toBeTruthy();
  });
  ```

- _We need to call the **ngOnInit()** again using **fixture.detectChanges()** so that the changes will also occur on the HTML._
