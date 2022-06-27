# (1) Introduction #

Angular is js framework to build Single-Page-Application\
Angular version: AngularJS(V1), after V2 all are same, V3 skipped.\
\
Install Angular:\
```sudo npm install -g @angular/cli```\
\
Create project:\
```ng new project_name```\
\
Run project:\
```ng serve -o```\
\
Add Bootstrap:\
```npm install --save bootstrap@3```\
and add style to angular.json

# (2) The Basics #

###@Component###\
-It is Decorator, It has selector,template or templateUrl (necessary),styles,styleUrls\
-bootstrap => load in index.html, other components in declaration, other modules in imports\
-string interpolation {{}} in between any expression returning string\
-property binding `[property]="variable"`\
-event binding `(event)="func($event)"`\
-two way binding `[(ngModel)]="variable"`\

{{{<p *ngIf="true; else localRef"></p>\
<ng-template #localRef></ng-template>`}}}\

`<p *ngFor="let item of array;let i= index">`\

`<p [ngStyle]="{backgroundColor:'red'}">`\
`<p [ngClass]="{className:expresion}"`\

debugging = webpack/./src\

(3) Components & Databinding

Custom Property binding:\
parent: `<p [prop]="var"></p>`\
child: `@Input('prop') var2;`\

Custom Event binding:\
parent: `<p (customE)="doSomething($event)"></p>`\
child:\
`@Output('customE') customEvnt = new EventEmitter<type>();<br />
onClick(){<br />
  cursomEvnt.emit(data);<br />
}`

View Encapsulation:\
@Component({encapsulation:ViewEncapsulation.Emulated})
now style of this component will apply to global.

ViewChild:
<p #localRef></p>
@ViewChild('localRef') var;  // this var type is ElementRef,set After ngAfterViewInit,not ngInit

ContentChild:
parent: <app-child>...<p #localRef></p>...</app-child>   // we can use this localref in both component.
child: <>...<ng-content></ng-content>...</>
child ts: @ContentChild('localRef') var;     // this will be set after ngAfterContentInit

NgContent:
parent: <app-component>somehtml</app-component>
child: <>...<ng-content></ng-content>...</>

LifeCycle:
ngOnChanges,ngOnInit,ngDoCheck,ngOnDestroy
ngAfterContentInit,ngAfterContentChecked,ngAfterViewInit,ngAfterViewChecked


<------------------------------------------------------------------------------------------->
(4) Directives
<------------------------------------------------------------------------------------------->

attribute directive vs structural directive
* structural directive,it change the dom
can not use two structural directive on one element.

<p [ngSwith]='value'>
 <p *ngSwitchCase="1">One</p>
 <p *ngSwitchCase="2">Two</p>
 <p *ngSwitchCase="3">Three</p>
</p>

custom directive:
@Directive({selector:'[drname]'});
constructor(private el:ElementRef){}
ngOnInit(){el.nativeElement.style.backgroundColor='red'}

better aproach:
constructor(private el:ElementRef,private r:Renderer2){}
ngOnInit(){r.setStyle(el.nativeElement,'background-color','red'}

using hostlistner&hostbinding:
@HostBinding('style.backgroundColor') var;
@HostListner('mouseenter') fun1(){ var='blue' }
@HostListner('mouseleave') fun2(){ var='transparent' }

<p *ngIf="var"></p>   // is same as
<ng-template [ngIf]="var"></ng-template>

//todo: video 101

<------------------------------------------------------------------------------------------->
(5) Services & DI
<------------------------------------------------------------------------------------------->


command: ng g s service_name

Basic Service:
@Injectable({providedIn:root})
export class MyService{
  logMethod(){}
}
use in component:
constructor(private mySerVar:MyService){}
this.mySerVar.logMethod();

In above example => creates only one instance of service and provides to all component.
@Component({providers:[MyService]})  => only for this and it's child components.
@NgModule({providers:[MyService]})  => all component of this module

we can create event emitter variable in service and emit from component and subscribe to event emitter anywhere

<------------------------------------------------------------------------------------------->
(6) Routing
<------------------------------------------------------------------------------------------->

in ng module
const appRoutes:Routes = [
  { path:'',component:Homepage },
  { path:'users',component:UserComponent },
  { path:'/404',component:404Component },
  { path:'**',redirectTo:'/404' }
]
imports:[RouterModule.forRoot(appRoutes)]

in app comp =>  <router-outlet></router-outlet>
in links => <a routerLink="/">Home</a> or [routerLink]="['/users','id']"
routerLinkActive="className" 
[routerLinkActiveOption]="{exact:true}"
[queryParams]="{allowEdit:'1'}"
fragment="loading"

programatically navigate:
=>we can add Router,ActivatedRoute to constructor
router.navigate(['servers'],{relativeTo:activeRoute,
    queryParams:{allowEdit:'1'},fragment:'loading',queryParamsHandling:'preserve'})

{path:'/user/:id',component:any}
get params: activeRoute.snapshot.params["id"];
get queryParams: activeRoute.snapshot.queryParams;
get fragment: activeRoute.snapshot.fragment;
or : activeRoute.params.subscribe((newPara)=>dothing(newPara))

//TODO: route guard

<------------------------------------------------------------------------------------------->
(7) Observables
<------------------------------------------------------------------------------------------->

interval(1000).subscribe(i=>clg(i));

custom observable:
const customObs = Observable.create(observer=>{
  let count=0;
  setInterval(()=>{
    observer.next(count);
    // observer.error(new Error("this is error msg"));
    // observer.complete();
    count++;
  },1000)
});

customObs.subscribe(data=>{
  console.log(data);
},error=>{
  console.log(error.message);
},()=>{
  console.log("completed");
});

operators:
observable.pipe(filter(data=>{
  return data%2==0?true:false;
}),map(data=>{
  return "data is "+data;
})).subscribe(data=>{
  console.log(data);
})

eventEmmiter: .emit(data),type: EventEmitter()
subject:  .next(data)
<------------------------------------------------------------------------------------------->
(8) Forms
<------------------------------------------------------------------------------------------->
template driven:
<form (ngSubmit)="onSubmit(f)" #f="ngForm" >
<input name="email" ngModel email >
onSubmit(myForm :ngForm){}

we can use ngModel without binding,one way,and also two way binding.
classes we can add style: ng-valid,ng-invalid,ng-touched,ng-dirty,...

<input #uname="ngModel" >
<p *ngIf="uname.invalid && uname.touched" >invalid email</p>
for group: <div #mygroup="ngModelGroup" ></div>

@ViewChild('formref') myForm:NgForm;
myForm.setValue({fullObj of form})
myForm.patchValue({only field want to update})
myForm.reset();

Reactive aproach:

myForm = new FormGroup({
  'username':new FormControl('defalut',Validators.required),
  'email': new FormControl(null,[Validators.required,Validators.email])
})
inHTML:
<form [formGroup]="myForm" (ngSubmit)="onSubmit()">
<input formControlName="username" >
<input formControlName="email" >
<p *ngIf="myForm.get('username').invalid">Invalid data</p>

//TODO Custom Validator

<------------------------------------------------------------------------------------------->
(9) Pipes
<------------------------------------------------------------------------------------------->

{{ myDate | date:'fullDate':...| uppercase }}

custom pipe:
@Pipe({
 name: 'filter',
 pure: true
})
export class myPipe implements PipeTransform{
 transform(value:any){
  return value.substr(0,5);
 }
}
async pipe useful

<------------------------------------------------------------------------------------------->
(10) Http
<------------------------------------------------------------------------------------------->


<------------------------------------------------------------------------------------------->
(11) Authentication
<------------------------------------------------------------------------------------------->


<------------------------------------------------------------------------------------------->
(12) Optimization & ngModules
<------------------------------------------------------------------------------------------->


<------------------------------------------------------------------------------------------->
(13) Deployment
<------------------------------------------------------------------------------------------->


<------------------------------------------------------------------------------------------->
(14) Animation & Testing
<------------------------------------------------------------------------------------------->

