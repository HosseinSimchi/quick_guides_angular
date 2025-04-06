# ANGULAR V19.

- src

  - account
    - account.routes.ts
  - app

    - components
    - app.routes.ts

      ```javascript
      import { Routes } from "@angular/router";
      import { authGuard } from "../shared/guards/auth/auth.guard";
      import { accessUserGuard } from "../shared/guards/user/access-user.guard";

      export const ApplicationRoutes: Routes = [
        {
          path: "layout",
          loadComponent: () =>
            import("./components/layout/layout.component").then(
              (m) => m.LayoutComponent
            ),
          children: [
            {
              path: "home",
              loadComponent: () =>
                import("./components/oas/home.component").then(
                  (m) => m.HomeComponent
                ),
            },
            {
              path: "settings",
              loadComponent: () =>
                import("./components/tags/settings.component").then(
                  (m) => m.SettingsComponent
                ),
            },
            {
              path: "account",
              loadComponent: () =>
                import("./components/user-management/account.component").then(
                  (m) => m.AccountComponent
                ),
              canActivate: [accessUserGuard],
            },
          ],
        },
        {
          path: "",
          redirectTo: "layout/oas",
          pathMatch: "full",
        },
      ];
      ```

  - root

    - RootComponent
    - root.config.ts

      ```javascript
      import {
        ApplicationConfig,
        provideZoneChangeDetection,
      } from "@angular/core";
      import { provideRouter } from "@angular/router";

      import { routes } from "./root.routes";
      import { provideAnimationsAsync } from "@angular/platform-browser/animations/async";
      import {
        provideHttpClient,
        withInterceptors,
      } from "@angular/common/http";
      import { authInterceptor } from "../shared/interceptors/auth.interceptor";
      import { provideToastr } from "ngx-toastr";

      export const rootConfig: ApplicationConfig = {
        providers: [
          provideZoneChangeDetection({ eventCoalescing: true }),
          provideHttpClient(withInterceptors([authInterceptor])),
          provideRouter(routes),
          provideAnimationsAsync(),
        ],
      };
      ```

    - route.routes.ts

      ```javascript
      import { Routes } from "@angular/router";
      import { NotFoundComponent } from "../shared/not-found/not-found.component";
      import { authGuard } from "../shared/guards/auth/auth.guard";
      import { ApplicationRoutes } from "../app/app.routes";

      export const routes: Routes = [
        {
          path: "",
          redirectTo: "auth", // Redirect to the login page by default
          pathMatch: "full",
        },
        {
          path: "auth",
          loadComponent: () =>
            import("../account/login/login.component").then(
              (c) => c.LoginComponent
            ),
        },
        {
          path: "app",
          loadComponent: () =>
            import("../app/app.component").then((c) => c.AppComponent),
          canActivate: [authGuard],
          children: ApplicationRoutes,
        },
        {
          path: "404",
          component: NotFoundComponent,
        },
        {
          path: "**",
          redirectTo: "404",
        },
      ];
      ```

  - shared

    - components
    - guards

      - auth.guard.ts

        ```javascript
        import { inject } from "@angular/core";
        import { CanActivateFn, Router } from "@angular/router";
        import { TokenService } from "../../services/token/token.service";
        import { AuthService } from "../../services/auth/auth.service";

        export const authGuard: CanActivateFn = (route, state) => {
          const router = inject(Router);
          const token = inject(TokenService).getToken();
          const authService = inject(AuthService);
          if (token) {
            return true;
          } else {
            authService.logout();
            router.navigate(["auth"], { replaceUrl: true });
            return false;
          }
        };
        ```

      - access-user.guard.ts:

        ```javascript
        import { inject } from "@angular/core";
        import { CanActivateFn, Router } from "@angular/router";
        import { AuthService } from "../../services/auth/auth.service";
        import { TokenService } from "../../services/token/token.service";
        import { Role } from "../../enums/user-management.enums";
        export const accessUserGuard: CanActivateFn = (route, state) => {
          const router = inject(Router);
          const authService = inject(AuthService);
          const tokenService = inject(TokenService);
          const savedToken = tokenService.getToken();
          const parsedToken = tokenService.decodeToken(
            savedToken ? savedToken : ""
          );
          if (parsedToken.role.includes(Role.Admin)) {
            return true;
          } else {
            authService.logout();
            router.navigate(["/"], { replaceUrl: true });
            return false;
          }
        };
        ```

    - interceptors

      - auth.interceptor.ts

        ```javascript
        import { HttpInterceptorFn } from "@angular/common/http";
        import { TokenService } from "../services/token/token.service";
        import { inject } from "@angular/core";
        export const authInterceptor: HttpInterceptorFn = (req, next) => {
          const savedToken = inject(TokenService);
          const token = savedToken.getToken();
          let authReq: any;
          if (token) {
            authReq = req.clone({
              setHeaders: {
                Authorization: `Bearer ${token}`,
              },
            });
            return next(authReq);
          }

          return next(req);
        };
        ```

    - routes
    - services
    - validators
