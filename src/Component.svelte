<script>
  import { getContext , onDestroy} from "svelte";
  import CellOptions from "../../bb_super_components_shared/src/lib/SuperTableCells/CellOptions.svelte";
  
  const { styleable, Provider, Block, BlockComponent } = getContext("sdk");
  const component = getContext("component");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const labelPos = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const formApi = formContext?.formApi;

  export let valueType = "string"
  export let field;
  export let numfield
  export let onChange
  export let validation
  export let numvalidation
  export let controlType = "select"
  
  export let customButtons = []

  export let buttons = [];

  export let label;
  export let span = 6;
  export let placeholder
  export let defaultValue
  export let disabled
  export let readonly

  export let icon

  export let optionsSource
  export let datasource
  export let valueColumn
  export let labelColumn
  export let colorColumn
  export let iconColumn
  export let limit
  export let sortColumn
  export let sortOrder
  export let filter
  export let customOptions = []
  export let optionsArrangement
  export let useOptionColors
  export let useOptionIcons
  export let optionsViewMode
  export let autocomplete
  export let addNew
  export let onAddNew

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema

  let cellState
  
  $: formStep = formStepContext ? $formStepContext || 1 : 1;

  $: valueType == "string" 
  ? formField = formApi?.registerField(
    field,
    "options",
    defaultValue,
    disabled,
    readonly,
    validation,
    formStep
  )
  : formField = formApi?.registerField(
    numfield,
    "number",
    defaultValue,
    disabled,
    readonly,
    numvalidation,
    formStep
  )

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
  });

  $: cellOptions = { 
      disabled,
      readonly : readonly || disabled,
      placeholder: placeholder || "Choose Option", 
      defaultValue,
      autocomplete,
      addNew,
      padding: "0.5rem",
      error: fieldState.error,
      controlType,
      optionsArrangement,
      optionsSource,
      datasource,
      filter,
      sortColumn,
      sortOrder,
      limit,
      valueColumn,
      labelColumn,
      colorColumn,
      iconColumn,
      useOptionColors,
      useOptionIcons,
      optionsViewMode,
      customOptions,
      role: "formInput", 
      icon,
    }

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "flex-direction": labelPos == "left" ? "row" : "column",
      gap: labelPos == "left" ? "0.85rem" : "0rem",
      "grid-column": labelPos ? "span " + span : "span 1",
      "--label-width":
        labelPos == "left" ? (labelWidth ? labelWidth : "6rem") : "auto",
    },
  };

  const handleChange = ( newValue ) => {
    onChange?.({value: newValue ? newValue : null });
    fieldApi?.setValue( valueType == "string" ? newValue ? newValue : null  : Number ( newValue ? newValue : null ) );
  }
  
  onDestroy(() => {
    fieldApi?.deregister()
    unsubscribe?.()
  })
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<Block>
  <div class="superField" use:styleable={$component.styles}>

    {#if label}
      <label for={$component.id} class="superlabel">
        {label}
          {#if fieldState.error}
            <div class="error">
              <span>{fieldState.error}</span>
            </div>
          {/if}
      </label>
     {/if}
    
    <div class="inline-cells">
      <CellOptions
        id={$component.id}
        bind:cellState
        {cellOptions}
        {fieldSchema}
        value={fieldState.value}
        on:change={(e) => handleChange(e.detail)}
        on:blur={cellState.lostFocus}
        on:addNew={ (e) => onAddNew?.({value: e.detail})}
      />

      {#if customButtons && buttons?.length}
        <div
          class="spectrum-ActionGroup spectrum-ActionGroup--compact spectrum-ActionGroup--sizeM"
        >
          <Provider data={ { value : fieldState.value }} >
            {#each buttons as { text, onClick , quiet, disabled, type }}
              <BlockComponent
                type = "plugin/bb-component-SuperButton"
                props = {{
                  quiet,
                  disabled, 
                  size: "M",
                  text,
                  onClick,
                  emphasized : true,
                  selected: type == "cta"
                }}>
                </BlockComponent>
              {/each}
          </Provider>
        </div>
      {/if}
    </div>
    
  </div>
</Block>

<style>
  .superField {
    display: flex;
    align-items: stretch;
    justify-content: stretch;
    min-width: 0;
  }

  .superField:focus {
    outline: none;
  }
  .superlabel {
    display: flex;
    justify-content: space-between;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    min-width: var(--label-width);
    max-width: var(--label-width);
    font-size: 12px;
    line-height: 1.75rem;
    font-weight: 400;
    color: var(--spectrum-global-color-gray-700);
  }

  .inline-cells {
    flex: 1;
    display: flex;
    justify-items: stretch;
    height: 2rem;
  }
  .error {
    font-size: 12px;
    line-height: 1.75rem;
    color: var(--spectrum-global-color-red-700);
  }
</style>


