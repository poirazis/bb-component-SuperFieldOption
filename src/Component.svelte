<script>
  import { getContext, onDestroy } from "svelte";
  import CellOptions from "../../bb_super_components_shared/src/lib/SuperTableCells/CellOptions.svelte";
  import "../../bb_super_components_shared/src/lib/SuperFieldsCommon.css";

  const { styleable, Provider, Block, BlockComponent } = getContext("sdk");
  const component = getContext("component");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const groupDisabled = getContext("field-group-disabled");
  const labelWidth = getContext("field-group-label-width");
  const formApi = formContext?.formApi;

  export let valueType = "string";
  export let field;
  export let numfield;
  export let onChange;
  export let debounced;
  export let debounceDelay;
  export let validation;
  export let numvalidation;
  export let controlType = "select";

  export let customButtons = [];

  export let buttons = [];

  export let fieldLabel;
  export let labelPosition;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;

  export let icon;

  export let optionsSource;
  export let datasource;
  export let valueColumn;
  export let labelColumn;
  export let colorTemplate;
  export let iconTemplate;
  export let limit;
  export let sortColumn;
  export let sortOrder;
  export let filter;
  export let prefetch;
  export let cache;
  export let fullTable;
  export let columnList;
  export let customOptions = [];
  export let optionsArrangement;
  export let useOptionColors;
  export let optionsIcon;
  export let optionsViewMode;
  export let autocomplete;
  export let addNew;
  export let onAddNew;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;

  let cellState;
  let context = {};

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: label = fieldLabel || fieldSchema?.name;
  $: labelPos = labelPosition ? labelPosition : groupLabelPosition || "left";

  $: valueType == "string"
    ? (formField = formApi?.registerField(
        field,
        "options",
        defaultValue,
        disabled,
        readonly,
        validation,
        formStep
      ))
    : (formField = formApi?.registerField(
        numfield,
        "number",
        defaultValue,
        disabled,
        readonly,
        numvalidation,
        formStep
      ));

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
    context = { value: fieldState.value };
  });

  $: cellOptions = {
    disabled: disabled || groupDisabled || fieldState?.disabled,
    readonly: readonly || fieldState?.readonly,
    placeholder: placeholder || "Choose Option",
    debounce: debounced ? debounceDelay : false,
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
    prefetch,
    cache,
    fullTable,
    columnList,
    valueColumn,
    labelColumn,
    colorTemplate,
    iconTemplate,
    useOptionColors,
    optionsIcon,
    optionsViewMode,
    customOptions,
    role: "formInput",
    icon,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "flex-direction": labelPos == "left" ? "row" : "column",
      gap: labelPos == "left" ? "0.5rem" : "0rem",
      "grid-column": labelPos ? "span " + span : "span 1",
      "--label-width":
        labelPos == "left" ? (labelWidth ? labelWidth : "6rem") : "auto",
    },
  };

  const handleChange = (newValue) => {
    onChange?.({ value: newValue ? newValue : null });
    console.log(newValue);
    fieldApi?.setValue(
      valueType == "string"
        ? newValue
          ? newValue
          : null
        : Number(newValue ? newValue : null)
    );
    context = { value: newValue };
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<Block>
  <div class="superField" use:styleable={$component.styles}>
    {#if label}
      <label for="superCell" class="superlabel" class:left={labelPos == "left"}>
        {label}
        {#if fieldState.error}
          <div class="error" class:left={labelPos == "left"}>
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
        on:addNew={(e) => onAddNew?.({ value: e.detail })}
      />

      {#if customButtons && buttons?.length}
        <Provider data={context}>
          <div
            class="spectrum-ActionGroup spectrum-ActionGroup--compact spectrum-ActionGroup--sizeM"
          >
            {#each buttons as { text, onClick, quiet, type, icon }}
              <BlockComponent
                type="plugin/bb-component-SuperButton"
                props={{
                  quiet,
                  disabled,
                  size: "M",
                  text,
                  onClick,
                  icon,
                  emphasized: true,
                  selected: type == "cta",
                }}
              ></BlockComponent>
            {/each}
          </div>
        </Provider>
      {/if}
    </div>
  </div>
</Block>
