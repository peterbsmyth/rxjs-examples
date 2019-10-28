# RxJs Examples
## Examples for Devs who cant async good and want to do other stuff good too

## Intro

## Table of Contents
[Example 1: map, switchMap, and custom operator ofType]()  
[Example 2: catchError]()  
[Example 3: withLatestFrom, filter, switchMap]()  
[Example 4: retry]()  
[Example 5: retryWhen, delay, scan]()  
[Example 6: drawing shapes with combineLatest]()  
[Example 7: GitHub Search](https://github.com/ecmadao/rxjs-example)  
[Example 8: Playground](https://github.com/JayKan/RxJS-Playground)  

  
## Example 1: map, switchMap, and custom operator ofType
```
login$ = createEffect(() =>
  this.actions$.pipe(
    ofType(UserActions.getUser),
    switchMap(() => this.apiService.getCurrentUser()),
    map((user) => UserActions.getUserComplete({ user }))
  )
);
```

## Example 2: catchError
```
validate$: Observable<Action> = createEffect(() =>
  this.actions$.pipe(
    ofType(UserActions.validateLogin),
    switchMap((res: any) => this.auth.getAccessToken(res.login).
      pipe(
        map((response) => UserActions.validateLoginComplete()),
        catchError(errorHandler(UserActions.validateLoginError))
      )
    )
  )
);
```

## Example 3: withLatestFrom, filter, switchMap
```
deleteDraftComplete$ = createEffect(() =>
  this.actions$.pipe(
    ofType(MyExpensesActions.deleteDraftComplete),
    withLatestFrom(this.store$),
    filter(([action, storeState]) => action.expense.id === storeState.dashboard.newExpense.receiptData['id']),
    switchMap(() => [
      NewExpenseActions.cancelComplete(),
      JobCodeActions.resetJobCode(),
      ExpenseTypeActions.resetExpenseType()
    ])
 )
);
```

## Example 4: retry
```
getReceiptData$ = createEffect(() =>
  this.actions$.pipe(
    ofType(NewExpenseActions.savePhotoComplete),
    switchMap(res =>
      this.apiService.getOneExpenseReceipt(res.tokenData).pipe(retry())
    ),
    map(res =>
      NewExpenseActions.getExpenseDataComplete({ expenseData: res })
    )
  )
);
```

## Example 5: retryWhen, delay, scan
```
getReceiptData$ = createEffect(() =>
  this.actions$.pipe(
    ofType(NewExpenseActions.savePhotoComplete),
     switchMap(res =>
      this.apiService.getOneExpenseReceipt(res.tokenData)
      .pipe(
        retryWhen(error$ => {
          return error$.pipe(
            delay(2000),
            scan((count, err) => {
              if (count > 10) {
                throw err;
              } else {
                return count += 1;
              }
            }, 0));
        }),
        map(response => NewExpenseActions.getExpenseDataComplete({ expenseData: response })),
        catchError(errorHandler(NewExpenseActions.getExpenseDataError))
      )
    )
  ),
);
```

## Example 6: Drawing Shapes with combineLatest
[Link](https://jsbin.com/padutujasu)
  
## Example 7: GitHub Search
[Link](https://github.com/ecmadao/rxjs-example)

## Example 8: Playground
[Link](https://github.com/JayKan/RxJS-Playground)
