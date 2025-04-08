## If you have two components used in the same HTML file, when running the test file associated with the HTML file, you must provide all the dependency requirements of the two components used (by selector), otherwise you will get an error.

### Example

- HTML file:

  ```html
  <div class="grid grid-cols-2 gap-4 bg-gray-100">
    <div class="bg-gray-100 rounded-lg shadow-lg shadow-gray-600/50">
      <app-first-component></app-first-component>
    </div>
    <div class="bg-gray-100 rounded-lg shadow-lg shadow-gray-600/50">
      <app-second-compoennt></app-second-compoennt>
    </div>
  </div>
  ```

  _Suppose that both the first and second components are dependent on **HttpClient** dependency. In this case, we need to provide the HttpClient mocked dependency in the test component. Instead, we use the following method because when the number of dependencies is large, providing all of them will be very difficult and illogical._

- .spec.ts file:

  ```javascript
  @Component({
    selector: "app-first-component",
    standalone: true,
    template: "<div></div>",
  })
  class MockFirstComponent {}

  @Component({
    selector: "app-second-component",
    standalone: true,
    template: "<div></div>",
  })
  class MockSecondComponent {}
  ```

  ```javascript
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [CurrentComponent],
    })
      .overrideComponent(CurrentComponent, {
        remove: { imports: [FirstComponent, SecondComponent] },
        add: {
          imports: [MockFirstComponent, MockSecondComponent],
        },
      })
      .compileComponents();

    fixture = TestBed.createComponent(CurrentComponent);
    component = fixture.componentInstance;
    el = fixture.debugElement;

    fixture.detectChanges();
  });
  ```

### Deep Integration tests

- HTML
  ```html
  <div class="grid grid-cols-2 gap-4 bg-gray-100">
    <div class="bg-gray-100 rounded-lg shadow-lg shadow-gray-600/50">
      <app-first-component></app-first-component>
    </div>
    <div class="bg-gray-100 rounded-lg shadow-lg shadow-gray-600/50">
      <app-second-compoennt></app-second-compoennt>
    </div>
  </div>
  ```
- Deep integration test (If the components used **do not** have dependencies and dependency injection)

  ```javascript
  it("#deep UT - should create an instance of FirstComponent", () => {
    const tag = el.query(By.directive(FirstComponent));

    expect(tag.componentInstance).toBeTruthy();
  });

  it("#deep UT - should create an instance of SecondComponent", () => {
    const tag = el.query(By.directive(SecondComponent));

    expect(tag.componentInstance).toBeTruthy();
  });
  ```

- Deep integration test (If the components used **have** dependencies and dependency injection like **the above example**)

  ```javascript
  it("#deep UT - should create an instance of FirstComponent", () => {
    const tag = el.query(By.directive(MockFirstComponent));

    expect(tag.componentInstance).toBeTruthy();
  });

  it("#deep UT - should create an instance of SecondComponent", () => {
    const tag = el.query(By.directive(MockSecondComponent));

    expect(tag.componentInstance).toBeTruthy();
  });
  ```
