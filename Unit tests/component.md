### setup

```javascript
let component: CustomComponent;
let fixture: ComponentFixture<CustomComponent>;
let el: DebugElement;
```

### Definition of mocked services (we write each of the functions required for testing inside quotation marks)

```javascript
let customServiceSpy: any;

beforeEach(async () => {
  customServiceSpy = jasmine.createSpyObj(["get", "getInitialValues"]);

  await TestBed.configureTestingModule({
    imports: [CustomComponent],
    providers: [
      {
        provide: CustomService,
        useValue: customServiceSpy,
      },
    ],
  }).compileComponents();

  fixture = TestBed.createComponent(CustomComponent); // Creating a wrapper for a component
  component = fixture.componentInstance; // Creating a component instance to test the TS file

  el = fixture.debugElement; // For use in testing elements
  fixture.detectChanges(); // Calling the ngOnInit method to apply the changes
});
```

### Checking whether a method has been called

```javascript
it("should customMethod() has been called", () => {
  spyOn(component, "customMethod");
  component.ngOnInit();

  expect(component.customMethod).toHaveBeenCalled();
});
```

### Checking whether a service is properly subscribed

```javascript
it("should subscribe customService.get()", () => {
  spyOn(component, "handleSuccess");
  customService.get.and.returnValue(of(true));

  component.getDataByService();

  expect(component.handleSuccess).toHaveBeenCalledWith(true);
});
```

### Checking whether a service is NOT subscribed

```javascript
it("should NOT subscribe customService.get()", () => {
  spyOn(component, "handleSuccess");
  spyOn(console, "error");

  customService.get.and.returnValue(throwError(() => new Error("Fake!")));
  component.getDataByService();

  expect(component.handleSuccess).not.toHaveBeenCalledWith(true);
  expect(console.error).toHaveBeenCalled();
});
```
