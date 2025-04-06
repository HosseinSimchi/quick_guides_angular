### customize material angular

- inside the custom-theme.scss

  ```scss
  @use "@angular/material" as mat;

  html {
    @include mat.theme(
      (
        color: (
          theme-type: light,
          primary: mat.$rose-palette,
          tertiary: mat.$rose-palette,
        ),
        typography: Roboto,
        density: 0,
      )
    );
    @include mat.form-field-overrides(
      (
        filled-caret-color: #4c0519,
        filled-focus-active-indicator-color: #4c0519,
        filled-focus-label-text-color: #4c0519,
        filled-label-text-color: #4c0519,
        filled-container-color: #f9fafb,
        filled-hover-active-indicator-color: #4c0519,
        filled-hover-label-text-color: #4c0519,
        // hover-state-layer-opacity: #f9fafb,
        outlined-label-text-color: #4c0519,
        filled-disabled-label-text-color: gray,
        filled-disabled-input-text-color: gray,
        filled-disabled-active-indicator-color: gray,
        disabled-input-text-placeholder-color: gray,
        select-disabled-option-text-color: gray,
        filled-input-text-color: #4c0519,
        filled-input-text-placeholder-color: #4c0519,
      )
    );
    @include mat.table-overrides(
      (
        background-color: white,
        header-headline-color: #4c0519,
        row-item-outline-color: #4c0519,
        row-item-label-text-color: #4c0519,
      )
    );
    @include mat.select-overrides(
      (
        panel-background-color: #f9fafb,
        enabled-trigger-text-color: #4c0519,
        placeholder-text-color: #4c0519,
        enabled-arrow-color: #4c0519,
        focused-arrow-color: #4c0519,
        disabled-trigger-text-color: gray,
      )
    );
    @include mat.dialog-overrides(
      (
        container-color: #f9fafb,
        subhead-color: #f9fafb,
      )
    );
    @include mat.paginator-overrides(
      (
        container-background-color: #f9fafb,
        container-size: 16px,
      )
    );
  }
  ```
