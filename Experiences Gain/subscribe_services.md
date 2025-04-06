```javascript
  getDataByService() {
    this.myInjectedService.get()?.subscribe({
      next: (response: any) => this.handleInjectedServiceSuccess(response),
      error: (error: any) => console.error(error),
    });
  }
```
