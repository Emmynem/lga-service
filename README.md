# lga-service
 All the state in Nigeria and their respective local goverment areas in an array accessible by the Angular 13 service

# How to use
## Import the service after pasting the TypeScript in your src folder and also the form classes i.e.
```
import { LgasService } from '../lgas.service';
import { FormControl, FormGroup, FormArray, FormBuilder, Validators } from '@angular/forms';
```

## In yout html component, this is how your inputs will be like
```
<label for="city">City: </label>
<select id="city" type="text" class="form-control" formControlName="city">
    <option value="" disabled selected>--Select Area--</option>
    <option *ngFor="let lga of lgas" value="{{lga}}">{{lga}}</option>
</select>

<label for="state">State: </label>
<select id="state" type="text" class="form-control" formControlName="state">
    <option value="" disabled selected>--Select State--</option>
    <option *ngFor="let state of all_states" value="{{state}}">{{state}}</option>
</select>
```

## Before your constructor, paste this (it can be your own personalized form)
```
public profileForm: FormGroup;
```

## In your constructor, do the following
```
constructor(private fb: FormBuilder, private lgasService: LgasService){
    this.profileForm = this.fb.group({
      city: ["", [Validators.required, Validators.minLength(2), Validators.maxLength(30)]],
      state: ['', [Validators.required, Validators.minLength(2), Validators.maxLength(30)]],
    });
}
```

## In your class after your contructor function and before your ngOnInit function, paste this
```
state = ""; // you'll assign this to your state form control
changed_state = ""; // this will be used to update the lgas via the current changed state
```

## After that create a function onChanges and call it in your ngOnInit function, this is how it'll look like
```
onChanges() {
  this.changed_state;
  this.profileForm.controls['state'].valueChanges.subscribe(selectedValue => {
    if (selectedValue != "" && selectedValue != this.changed_state && this.city.value != "") {
      this.profileForm.patchValue({
        city: ""
      }, { emitEvent: false });
    }
    this.changed_state = selectedValue;
    this.change_lgas(selectedValue);
  });

}
```

## Paste this line of code next after your onChanges function
```
lgas: string[] = [];
all_states: string[] = this.lgasService.listStates();

change_lgas (state : string){
  this.lgas = this.lgasService.changeLGA(state);
}

get city() {
  return this.profileForm.get('city') as FormControl;
}
```

That's all you can now change the lgas based on the state you select. Good luck, if you have any issue or need guidance just comment or drop a review.



