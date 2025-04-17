<script lang="ts">
	import type { ClassValue, } from 'svelte/elements';
	import type { Snippet } from 'svelte';
	
	interface Props extends Graphite.SvelteSpanElement {
		style?: string;
		styles?: Graphite.StyleValue;
		disabled?: boolean;
		bold?: boolean;
		italic?: boolean;
		centerAlign?: boolean;
		tableAlign?: boolean;
		minWidth?: number;
		multiline?: boolean;
		tooltip?: string | undefined;
	}

	let {
		class: className,
		style: styleName = "",
		styles = {},
		disabled = false,
		bold = false,
		italic = false,
		centerAlign = false,
		tableAlign = false,
		minWidth = 0,
		multiline = false,
		tooltip = undefined,
		children
	}: Props = $props();

	let extraStyles = $derived(Object.entries(styles)
		.flatMap((styleAndValue) => (styleAndValue[1] !== undefined ? [`${styleAndValue[0]}: ${styleAndValue[1]};`] : []))
		.join(" "));
</script>

<span
	class={["text-label", className, {
		disabled,
		bold,
		italic,
		multiline,
		"center-align": centerAlign,
		"table-align": tableAlign
	}]}
	style:min-width={minWidth > 0 ? `${minWidth}px` : ""}
	style={`${styleName} ${extraStyles}`.trim() || undefined}
	title={tooltip}
>
	{@render children?.()}
</span>

<style lang="scss" global>
	.text-label {
		line-height: 18px;
		white-space: nowrap;
		// Force Safari to not draw a text cursor, even though this element has `user-select: none`
		cursor: default;

		&.disabled {
			color: var(--color-8-uppergray);
		}

		&.bold {
			font-weight: 700;
		}

		&.italic {
			font-style: italic;
		}

		&.multiline {
			white-space: pre-wrap;
			margin: 4px 0;
		}

		&.center-align {
			text-align: center;
		}

		&.table-align {
			flex: 0 0 30%;
			text-align: right;
		}
	}
</style>
