# replace of rtf content

```
$rtf= file_get_contents('result.rtf');

$rtf = str_replace('[nickname]', $nickname, $rtf);
$rtf = str_replace('[secretWord]', $secretWord, $rtf);
$rtf = str_replace('[hoursInADay]', $hoursInADay, $rtf);
$rtf = str_replace('[hobby]', $hobby, $rtf);
$rtf = str_replace('[hobbyYears]', $hobbyYears, $rtf);
$rtf = str_replace('[hobbyAlphabet]', $hobbyAlphabet, $rtf);
$rtf = str_replace('[jokeFact]', $jokeFact, $rtf);

header('Content-type: application/msword');
header('Content-Disposition: attachment; filename=survey_result.rtf');
echo $rtf;
die();
```

#dispatch with multiparams in livewire 3
```
<!-- resources/views/livewire/filter-section.blade.php -->

<button 
  wire:click="$dispatch('apply-filter', { 
    offer_type_id: $wire.filters.offer_type.value,
    property_type_id: $wire.filters.property_type.value,
    sub_property_type_id: $wire.filters.sub_property_type.value,
  })"
>Apply</button>
```
#Get Carbon lastDay and firstDay of Month
```
$firstDayofPreviousMonth = Carbon::now()->startOfMonth()->subMonthsNoOverflow()->toDateString();

$lastDayofPreviousMonth = Carbon::now()->subMonthsNoOverflow()->endOfMonth()->toDateString();
```
# Dropdown menu with alpineJS
```
<div x-data="{ open: false, openedWithKeyboard: false }" class="relative"
@keydown.esc.window="open = false, openedWithKeyboard = false">
  <button type="button"
      class="p-2 text-gray-500 rounded-lg sm:flex hover:text-gray-900 hover:bg-gray-100"
      @click="open = ! open"
      aria-haspopup="true" @keydown.space.prevent="openedWithKeyboard = true"
      @keydown.enter.prevent="openedWithKeyboard = true"
      @keydown.down.prevent="openedWithKeyboard = true"
      :class="open || openedWithKeyboard ? 'text-neutral-900' : 'text-neutral-600'"
      :aria-expanded="open || openedWithKeyboard">
      <span class="sr-only">Apps</span>
        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"></svg>
  </button>

  <div class="fixed right-0 top-10 z-99 max-w-sm my-4 overflow-hidden text-base list-none bg-white divide-y divide-gray-100 rounded shadow-lg"
       x-cloak x-show="open || openedWithKeyboard" x-transition x-trap="openedWithKeyboard"
       @click.outside="open = false, openedWithKeyboard = false"
       @keydown.down.prevent="$focus.wrap().next()" @keydown.up.prevent="$focus.wrap().previous()"
       role="menu">
       <div class="block px-4 py-2 text-base font-medium text-center text-gray-700 bg-gray-50">Apps</div>
  </div>
```
# Detect Mobile or Desktop
in AppServiceProvider
```
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

use Illuminate\Pagination\Paginator;

use Illuminate\Support\Facades\Auth;

use Illuminate\Support\Facades\View;

use Illuminate\Support\Facades\DB;
use Blade;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
       Blade::if('detect', function ($value) {
      if(!empty($_SERVER['HTTP_USER_AGENT'])){
   $user_ag = $_SERVER['HTTP_USER_AGENT'];
   if(preg_match('/(Mobile|Android|Tablet|GoBrowser|[0-9]x[0-9]*|uZardWeb\/|Mini|Doris\/|Skyfire\/|iPhone|Fennec\/|Maemo|Iris\/|CLDC\-|Mobi\/)/uis',$user_ag)){
      return 'mobile';
   };
    }else{
    return 'desktop';
    }
});
}
```
and use it in Blade:
```
@detect("mobile")
class="mb-3 col-6 "
@else
class="mb-3 col-3 " 
@enddetect

    >
    <label for="exampleInputEmail1" class="form-label">Category</label>
    <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
  </div>
```

# Use SpatieRolePermission in Livewire
1. create Trait
```
<?php


namespace App\Traits\Admin;

use Illuminate\Support\Facades\Auth;
use Spatie\Permission\Exceptions\UnauthorizedException;

trait RoleOrPermissionSpatie
{
    public function handlePermission($roleOrPermission)
    {
        $authGuard = Auth::guard();
        if ($authGuard->guest()) {
            throw UnauthorizedException::notLoggedIn();
        }

        $rolesOrPermissions = is_array($roleOrPermission)
            ? $roleOrPermission
            : explode('|', $roleOrPermission);

        if (! $authGuard->user()->hasAnyRole($rolesOrPermissions) && ! $authGuard->user()->hasAnyPermission($rolesOrPermissions)) {
            throw UnauthorizedException::forRolesOrPermissions($rolesOrPermissions);
        }
    }
}

```
2. in your component add those Trait
```
namespace App\Http\Livewire\Admin;

use Livewire\Component;
...
...
use App\Traits\Admin\RoleOrPermissionSpatie;



class Programas extends Component
{
    use RoleOrPermissionSpatie;

    public function __construct()
    {
      $this->handlePermission('role1|role2|permission1');
    }
}


```
# Use flatpikr in blade component with alpineJS
```
<div wire:ignore x-data="datepicker(@entangle($attributes->wire('model')))" class="relative">    
    <x-input class="mt-1 block w-full" x-ref="myDatepicker" x-model="value" id="{{ rand() }}" />    
</div>

@once
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/flatpickr.min.css">
    <script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>

    <script>
        document.addEventListener('alpine:init', () => {
            Alpine.data('datepicker', (model) => ({
                value: model,
                init(){
                    this.pickr = flatpickr(this.$refs.myDatepicker, {dateFormat:'d/m/Y'})

                    this.$watch('value', function(newValue){
                        this.pickr.setDate(newValue);
                    }.bind(this));
                },
                reset(){
                    this.value = null;
                }
            }))
        })
    </script>
@endonce
```
```
<div class="mb-5" wire:ignore>        
    <x-label value="Date" />
    <x-datepicker wire:model="date" />
    <x-input-error for="date" class="mt-2" />
</div>  

<div class="mb-5" wire:ignore>        
    <x-label value="Today" />
    <x-datepicker wire:model="today" />
    <x-input-error for="today" class="mt-2" />
</div>
```

# Embeded MS Word (DOC/DOCX) in html or Blade

use offoiceappa aspx in iframe:
```
<iframe src='https://view.officeapps.live.com/op/embed.aspx?src=http://www.learningaboutelectronics.com/Articles/NP-modernization-act-new-york-state.doc' width='80%' height='565px' frameborder='0'> </iframe>
```

# How to embed PDF viewer in HTML
You could try embedding the PDF file in an 'object' tag. Here is an example of how to do this:

Example of adding a PDF file to the HTML by using the 'object' tag:
```
    <h1>PDF Example by Object Tag</h1>
    <object data="/uploads/media/default/0001/01/540cb75550adf33f281f29132dddd14fded85bfc.pdf" type="application/pdf" width="100%" height="500px">
      <p>Unable to display PDF file. <a href="/uploads/media/default/0001/01/540cb75550adf33f281f29132dddd14fded85bfc.pdf">Download</a> instead.</p>
    </object>
```
