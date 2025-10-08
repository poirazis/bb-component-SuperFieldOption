<script>
  import { getContext, onDestroy } from "svelte";
  import {
    CellOptions,
    CellOptionsAdvanced,
    SuperButton,
    SuperField,
  } from "@poirazis/supercomponents-shared";

  const { styleable, enrichButtonActions, Provider, builderStore } =
    getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupColumns = getContext("field-group-columns");
  const groupDisabled = getContext("field-group-disabled");
  const formApi = formContext?.formApi;

  export let field = "Option Field";
  export let onChange;
  export let debounced;
  export let debounceDelay = 50;
  export let validation;
  export let controlType = "select";
  export let role = "formInput";
  export let labelPosition = "fieldGroup";
  export let showDirty;

  export let buttons = [];

  export let label;
  export let span = 6;
  export let placeholder;
  export let helpText;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let autofocus;
  export let invisible = false;

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
  export let customOptions = [];
  export let direction;
  export let useOptionColors;
  export let optionsIcon;
  export let optionsViewMode;
  export let autocomplete;
  export let addNew;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value;

  $: multirow = controlType == "radio" && direction == "column";
  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos =
    groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition;

  $: formField = formApi?.registerField(
    field,
    "options",
    defaultValue,
    disabled,
    readonly,
    validation,
    formStep
  );
  $: value = fieldState?.value;
  $: error = fieldState?.error;

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
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
    error: fieldState?.error,
    controlType,
    direction,
    optionsSource,
    datasource,
    filter,
    sortColumn,
    sortOrder,
    limit,
    valueColumn,
    labelColumn,
    colorTemplate,
    iconTemplate,
    useOptionColors,
    optionsIcon,
    optionsViewMode,
    customOptions,
    role,
    icon,
    showDirty,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      display:
        invisible && !$builderStore.inBuilder
          ? "none"
          : $component.styles.normal.display,
      opacity: invisible && $builderStore.inBuilder ? 0.6 : 1,
      "grid-column": groupColumns ? `span ${span}` : "span 1",
      overflow: "hidden",
    },
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div use:styleable={$component.styles}>
  <Provider data={{ value }} />
  <SuperField
    {multirow}
    {labelPos}
    {labelWidth}
    {field}
    {label}
    {error}
    {helpText}
  >
    {#if controlType == "select" || controlType == "inputSelect"}
      <CellOptions
        {cellOptions}
        {fieldSchema}
        {value}
        {autofocus}
        on:change={(e) => {
          value = e.detail;
          onChange?.({ value: e.detail });
          fieldApi?.setValue(e.detail);
        }}
      />
    {:else}
      <CellOptionsAdvanced
        {cellOptions}
        {fieldSchema}
        {value}
        {autofocus}
        on:change={(e) => {
          onChange?.({ value: e.detail });
          fieldApi?.setValue(e.detail);
        }}
      />
    {/if}

    {#if buttons?.length}
      <div class="inline-buttons">
        {#each buttons as { text, onClick, quiet, disabled, type, size }}
          <SuperButton
            {quiet}
            {disabled}
            {size}
            {type}
            {text}
            on:click={enrichButtonActions(
              onClick,
              $allContext
            )({ value: fieldState.value })}
          />
        {/each}
      </div>
    {/if}
  </SuperField>
</div>
