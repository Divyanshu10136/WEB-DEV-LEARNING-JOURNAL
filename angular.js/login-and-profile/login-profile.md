**Author:** Divyanshu


## 1. Login Flow in ResumeFlow

The main practical flow covered in the session is a complete login-to-profile process:

```text
User enters email + password
          ↓
       Login API
          ↓
     Server returns token
          ↓
     Save token locally
          ↓
     Navigate to /profile
          ↓
   Profile requests user data
          ↓
   Interceptor adds token
          ↓
      Server verifies token
          ↓
     Profile data displayed
```

The PDF uses ResumeForge as the example and explains this complete flow using an Angular 13 style implementation. 

---

## 2. Auth Service

I can keep authentication-related API calls inside an `AuthService`.

The service:

* Sends login credentials.
* Receives the authentication token.
* Saves the token.
* Reads the stored token.
* Removes the token during logout.

Example:

```ts
@Injectable({
  providedIn: 'root'
})
export class AuthService {

  private api = 'https://api.resumeforge.com';

  constructor(private http: HttpClient) {}

  login(email: string, password: string) {
    return this.http.post(this.api + '/login', {
      email,
      password
    });
  }

  saveToken(token: string) {
    localStorage.setItem('token', token);
  }

  getToken() {
    return localStorage.getItem('token');
  }

  logout() {
    localStorage.removeItem('token');
  }
}
```

A useful design idea from the session is to keep each method focused on one responsibility. 

---

## 3. Login Component

The login component collects the user's credentials and calls the authentication service.

```ts
onLogin() {
  this.auth.login(this.email, this.password).subscribe(
    (res: any) => {
      this.auth.saveToken(res.token);
      this.router.navigate(['/profile']);
    },
    (err) => {
      this.error = 'Invalid email or password';
    }
  );
}
```

The important sequence is:

```text
Login API succeeds
       ↓
Token received
       ↓
Save token
       ↓
Navigate to profile
```

The token is saved inside the subscription callback because that is where the server response containing the token becomes available. 

### Login HTML

```html
<input [(ngModel)]="email" placeholder="Email">

<input
  [(ngModel)]="password"
  type="password"
  placeholder="Password"
>

<button (click)="onLogin()">Login</button>

<p *ngIf="error">{{ error }}</p>
```

This connects the form inputs with the component properties and displays an error when authentication fails. 

---

# 4. Profile Service

After login, the profile page needs to retrieve the user's information.

I can create a separate `ProfileService`:

```ts
@Injectable({
  providedIn: 'root'
})
export class ProfileService {

  private api = 'https://api.resumeforge.com';

  constructor(private http: HttpClient) {}

  getProfile() {
    return this.http.get(this.api + '/me');
  }
}
```

The important point is that this service does **not** manually add the authentication token. The interceptor handles that automatically. 

---

# 5. HTTP Interceptor

An interceptor sits between the application and HTTP requests.

Its job in this example is to check whether a token exists and, if it does, attach it to the request.

```text
Component
    ↓
Service
    ↓
HTTP Request
    ↓
Interceptor
    ↓
Add Authorization Header
    ↓
Backend
```

Example:

```ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  constructor(private auth: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler) {

    const token = this.auth.getToken();

    if (token) {
      const cloned = req.clone({
        setHeaders: {
          Authorization: 'Bearer ' + token
        }
      });

      return next.handle(cloned);
    }

    return next.handle(req);
  }
}
```

This means the profile service can simply request `/me` without repeatedly writing authentication-header code. 

---

# 6. Registering the Interceptor

The interceptor needs to be registered with Angular's HTTP system.

```ts
{
  provide: HTTP_INTERCEPTORS,
  useClass: AuthInterceptor,
  multi: true
}
```

After registration, HTTP requests pass through the interceptor automatically. 

---

# 7. Profile Component

The profile component calls the profile service when it is initialized.

```ts
export class ProfileComponent implements OnInit {

  user: any = null;

  constructor(private profile: ProfileService) {}

  ngOnInit() {
    this.profile.getProfile().subscribe((data) => {
      this.user = data;
    });
  }
}
```

The returned user information can then be displayed in the template:

```html
<div *ngIf="user">
  <h2>{{ user.name }}</h2>
  <p>{{ user.email }}</p>
</div>
```



---

# 8. Angular Routing

The application needs routes for the login and profile pages.

```ts
const routes: Routes = [
  {
    path: 'login',
    component: LoginComponent
  },
  {
    path: 'profile',
    component: ProfileComponent
  },
  {
    path: '',
    redirectTo: 'login',
    pathMatch: 'full'
  }
];
```

This gives the application:

```text
/login
    ↓
LoginComponent

/profile
    ↓
ProfileComponent

/
    ↓
Redirect to /login
```



---

# 9. Router Outlet

Angular needs a place where the selected route's component can appear.

I use:

```html
<router-outlet></router-outlet>
```

The routing system then places the correct component inside this outlet.

```text
URL
 ↓
Angular Router
 ↓
Matching Route
 ↓
Component
 ↓
<router-outlet>
```



---

# 10. Route Guard

At this stage, simply having a `/profile` route does not stop someone from directly typing the URL.

A **route guard** can later check whether a token exists before allowing the profile page to open.

```text
/profile
   ↓
Auth Guard
   ↓
Token available?
  ↙        ↘
Yes        No
 ↓          ↓
Profile    Login
```

The PDF identifies this as the next step for protecting the profile route. 

---

# 11. Styling with SCSS

The session also introduces **SCSS** for styling Angular components.

SCSS extends normal CSS with features such as:

* Variables
* Nesting
* Mixins
* `@extend`
* Functions
* Operators

Angular supports component stylesheets using the `.scss` extension. 

---

# 12. SCSS Variables

Instead of repeatedly writing the same colour, I can store it in a variable.

```scss
$primary: #c0272d;
$radius: 6px;
```

Then reuse it:

```scss
button {
  background: $primary;
  border-radius: $radius;
}
```

If I change `$primary`, the related styles can use the new value automatically. 

---

# 13. SCSS Nesting

SCSS allows related styles to be placed inside their parent selector.

```scss
.login-box {

  input {
    width: 100%;
    padding: 10px;
  }

  button {
    padding: 10px 16px;

    &:hover {
      background: darken($primary, 10%);
    }
  }
}
```

The `&:hover` represents the hover state of the button.

Nesting can make styles easier to organize around the HTML structure. 

---

# 14. SCSS Mixins

A **mixin** is a reusable block of SCSS styles.

For example:

```scss
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

I can use it with:

```scss
.login-box {
  @include flex-center;
}
```

This is useful when the same group of styles is needed in several places. 

---

# 15. Mixins with Parameters

Mixins can also receive values.

```scss
@mixin button($bg) {
  background: $bg;
  color: white;
  padding: 10px 16px;
  border-radius: 6px;
}
```

Then I can create different buttons:

```scss
.save-btn {
  @include button(#c0272d);
}

.cancel-btn {
  @include button(#6b7a8f);
}
```

This lets one reusable pattern produce different variations. 

---

# 16. `@extend`

`@extend` allows one selector to reuse the styling of another selector.

Example:

```scss
.btn {
  padding: 10px 16px;
  border-radius: 6px;
  border: none;
}

.save-btn {
  @extend .btn;
  background: $primary;
}

.cancel-btn {
  @extend .btn;
  background: #6b7a8f;
}
```

Both buttons inherit the common `.btn` styles and then add their own background. 

### Simple Difference

```text
@mixin
   ↓
Useful when styles need arguments or variations

@extend
   ↓
Useful when selectors share a common base style
```

---

# 17. SCSS Functions and Operators

SCSS can perform calculations and use functions.

Example:

```scss
.col {
  width: 100% / 3;
}
```

A colour function can also be used:

```scss
.button:hover {
  background: darken($primary, 10%);
}
```

The notes also mention functions such as:

```text
darken()
lighten()
rgba()
```



---

# 18. SCSS Placeholders

A placeholder begins with `%`.

Example:

```scss
%card-base {
  padding: 20px;
  border-radius: 6px;
  border: 1px solid #eee;
}
```

It can then be reused with `@extend`:

```scss
.profile-card {
  @extend %card-base;
}

.resume-card {
  @extend %card-base;
}
```

The placeholder itself isn't output as a standalone CSS selector; it exists to provide reusable styles. 

---

