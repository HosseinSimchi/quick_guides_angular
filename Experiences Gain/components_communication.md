## NGRX

## EventEmitter

- child component (**CustomComponent**)

  ```javascript
  @Output() clicked = new EventEmitter<void>();
  buttonClicked() {
    this.clicked.emit();
  }
  ```

  ```html
  <button (click)="buttonClicked()"></button>
  ```

- parent
  ```html
  <custom (clicked)="method()"></custom>
  ```

## Using subject / BehaviorSubject

- service (**CustomService**)
  ```javascript
  childsValue = new BehaviorSubject() < any > null;
  ```
- To change the value (**FirstComponent**)
  ```javascript
  this.customService.childsValue.next(2);
  ```
- To get the updated/latest value (**SecondComponent**)
  ```javascript
  this.data = this.customService.childsValue.subscribe({
    next: (response: any) => this.handleUpdatedValueSuccess(response),
  });
  ```
