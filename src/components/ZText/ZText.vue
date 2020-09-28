<script lang="ts">
import { h, defineComponent, Slot, onMounted } from 'vue';
import ztextify from './../../lib/ztext';

export default defineComponent({
  props: {
    as: {
      type: String,
      default: 'div',
      validator: (value: string): boolean => {
        const el = document.createElement(value);
        el.innerHTML = '.';
        return !!el.innerHTML;
      },
    },

    depth: {
      type: String,
      default: '1rem',
    },

    layers: {
      type: Number,
      default: 10,
    },

    perspective: {
      type: String,
      default: '500px',
    },

    fade: {
      type: Boolean,
      default: false,
    },

    direction: {
      type: String,
      default: 'both',
      validator: (value: string) => {
        return ['both', 'backwards', 'forwards'].includes(value);
      },
    },

    event: {
      type: String,
      default: 'none',
      validator: (value: string) => {
        return [
          'none',
          'pointer',
          'scroll',
          'scrollX',
          'scrollY',
        ].includes(value);
      },
    },

    eventRotation: {
      type: String,
      default: '30deg',
    },

    eventDirection: {
      type: String,
      default: 'default',
      validator: (value: string) => {
        return ['default', 'reverse'].includes(value);
      },
    },
  },

  setup(props, { slots }) {
    const attrs: Record<string, any> = {
      'data-z': true,
    };

    Object.keys(props)
      .filter((val: string) => val !== 'as')
      .forEach((val: string) => {
        attrs[`data-z-${val}`] = val;
      });

    const defaultSlot = (slots.default as Slot)();

    onMounted(() => {
      ztextify();
    });

    return () => {
      h(
        props.as,
        attrs,
        defaultSlot,
      );
    };
  },
});
</script>