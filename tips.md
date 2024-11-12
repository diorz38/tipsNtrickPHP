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
