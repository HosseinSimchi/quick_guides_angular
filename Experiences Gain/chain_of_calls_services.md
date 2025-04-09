## To give a list of IDs to a service and receive the output in the form of an array for each of the IDs.

#### _Without using this method, we have to call the desired service each time for each of the IDs and using loops, and if the number of members in the list is large, it will have a big impact on performance._

```javascript
  getData(ids: string[]): Observable<any[]> {
      return from(ids).pipe(
        mergeMap((id) =>
          this.httpClient.get("/api", {
            params: new HttpParams().set('id', id),
          })
        ),
        toArray() // gather all results into one array
      );
  }
```
