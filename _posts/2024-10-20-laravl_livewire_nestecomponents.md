---
layout: post
title:  "Laravel - Livewire. Nested components with live reaction to form input"
---

Livewire offers a powerful opportunity automatically react the user input. Here is an example to update dynamically a HTML-Tablestructure to a Form input.

[Livewire: Nesting Components](https://livewire.laravel.com/docs/nesting/ "Nesting components")

**The base component with sample random data for mount() and update()**

```
use Livewire\Component;
use Illuminate\Support\Str;

class Nested extends Component
{

    public $tablecontent;
    //Form variable must be declared for updated()-function
    public $myinput;

    public function mount()
    {
        $this->tablecontent = [
            ['age' => random_int(10, 80), 'content' => Str::random(random_int(7, 12))],
            ['age' => random_int(10, 80), 'content' => Str::random(random_int(7, 12))],
            ['age' => random_int(10, 80), 'content' => Str::random(random_int(7, 12))],
        ];
    }

    public function updated($property)
    {
        if ($property == 'myinput') {
            $this->tablecontent = [
                ['age' => random_int(10, 80), 'content' => $this->myinput . Str::random(random_int(7, 12))],
                ['age' => random_int(10, 80), 'content' => $this->myinput . Str::random(random_int(7, 12))],
                ['age' => random_int(10, 80), 'content' => $this->myinput . Str::random(random_int(7, 12))],
            ];
        }
    }

    public function render()
    {
        return view('mypackage::livewire.nested', ['tablecontent' => $this->tablecontent]);
    }
}

```
**The Bladeview with the nested Table component**
```
<div>
    <div class="field">
        <label class="label">Input</label>
        <div class="control">
            <input class="input" type="text" name="myinput" id="myinput" wire:model.live="myinput" />
        </div>
    </div>
    Table
    <livewire:livewire-tablenested :$tablecontent />
</div>
```
**Register the component (for example within the package service provider )**
```
    Livewire::component('livewire-nested', Nested::class);

```

**Create the nested table component (activate update with #[Reactive])**

```
use Livewire\Component;
use Livewire\Attributes\Reactive;

class Tablenested extends Component
{
    #[Reactive]
    public $tablecontent;

    public function render()
    {
        return view('basepackage::livewire.partials.tablenested', ['tablecontent' => $this->tablecontent]);
    }
}
```
**The view for the the nested table**
```
<div>
    <table class="table">
        <tr>
            <td>Name</td>
            <td>Age</td>
        </tr>
        @foreach ($tablecontent as $item)
            <tr>
                <td>{{ $item['content'] }}</td>
                <td>{{ $item['age'] }}</td>
            </tr>
        @endforeach
    </table>
</div>

```

**Register the nested component**
```
    Livewire::component('livewire-tablenested', Tablenested::class);
```





