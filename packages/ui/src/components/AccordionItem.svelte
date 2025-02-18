<!--
// Copyright © 2024 Hardcore Engineering Inc.
//
// Licensed under the Eclipse Public License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License. You may
// obtain a copy of the License at https://www.eclipse.org/legal/epl-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
//
// See the License for the specific language governing permissions and
// limitations under the License.
-->
<script lang="ts">
  import type { Asset, IntlString } from '@hcengineering/platform'
  import { AnySvelteComponent } from '../types'
  import { ComponentType } from 'svelte'
  import Icon from './Icon.svelte'
  import Label from './Label.svelte'
  import IconChevronRight from './icons/ChevronRight.svelte'

  export let label: IntlString | undefined = undefined
  export let title: string | undefined = undefined
  export let icon: Asset | AnySvelteComponent | ComponentType | undefined = undefined
  export let iconProps: any | undefined = undefined
  export let size: 'small' | 'medium' | 'large'
  export let nested: boolean = false
  export let isOpen: boolean = false
  export let selected: boolean = false
  export let bottomSpace: boolean = true
  export let counter: number | boolean = false
  export let duration: number | boolean = false
  export let fixHeader: boolean = false
  export let background: string | undefined = undefined
</script>

<div class="hulyAccordionItem-container default" class:nested>
  <button
    class="hulyAccordionItem-header default {size}"
    class:bottomSpace
    class:nested
    class:isOpen
    class:selected
    class:scroller-header={fixHeader}
    style:background-color={background ?? 'transparent'}
    on:click={() => {
      isOpen = !isOpen
    }}
  >
    <div
      class="hulyAccordionItem-header__label-wrapper {size === 'large' ? 'heading-medium-16' : 'font-medium-12'}"
      class:withIcon={size === 'medium' && icon !== undefined}
    >
      {#if size === 'large'}
        <div class="hulyAccordionItem-header__chevron">
          <Icon icon={IconChevronRight} size={'small'} />
        </div>
      {/if}
      {#if size !== 'small' && icon !== undefined}
        <div class="hulyAccordionItem-header__chevron">
          <Icon {icon} size={size === 'medium' ? 'small' : 'medium'} {iconProps} />
        </div>
      {/if}
      <span class="hulyAccordionItem-header__label">
        {#if label}<Label {label} />{/if}
        {#if title}{title}{/if}
        <slot name="title" />
      </span>
      {#if counter !== false}
        <span class="hulyAccordionItem-header__separator">•</span>
        <span class="hulyAccordionItem-header__counter">
          {#if typeof counter === 'number'}{counter}{/if}
          <slot name="counter" />
        </span>
      {/if}
      {#if duration !== false}
        <span class="hulyAccordionItem-header__separator">•</span>
        <span class="hulyAccordionItem-header__duration">
          {#if typeof duration === 'number'}{duration}{/if}
          <slot name="duration" />
        </span>
      {/if}
    </div>
    <div class="hulyAccordionItem-header__tools">
      <slot name="actions" />
    </div>
  </button>
  <div class="hulyAccordionItem-content">
    <slot />
  </div>
</div>
