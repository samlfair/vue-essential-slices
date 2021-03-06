<template>
	<ps-section
		:theme="theme.wrapper"
		classAttr="ps-testimonials"
	>
    <div class="ps__wrap">

        <div v-if="slice.items.length" class="ps__main grid grid--12">
          <div class="span-1-12">
            <div class="c-tabs js-tabs" data-tabs>
							<div class="c-tabs__tablist c-tabs__tablist--imgs span-1-12" role="tablist" data-tablist>
                <button
									v-for="(testimonial, i) in slice.items"
									:key="`testimonial-button-${i}`"
									:aria-selected="i === index"
									@click="index = i"
									class="c-tabs__tab"
									data-tab
								>
              		<prismic-image v-if="testimonial.client_logo" v-bind="theme.logo" :field="testimonial.client_logo" />
            		</button>
              </div>

                    <section
											class="c-tabs__tabpanel span-1-12" data-tabpanel
											v-for="(testimonial, i) in slice.items"
											:key="`testimonial-${i}`"
											:hidden="i !== index"
										>
                        <div class="ps-testimonials__text span-1-6">
													<div class="ps__head ps__head--left">
															<header class="ps__header">
																<ps-eyebrow v-if="slice.primary.eyebrow_headline" :theme="theme.eyebrow" :align="theme.align" :color="theme.color">
																	{{ $prismic.asText(slice.primary.eyebrow_headline) }}
																</ps-eyebrow>
                                <ps-title :field="slice.primary.title" :theme="theme.title" :align="theme.align" :color="theme.color" />

															</header>
															<ps-description :field="slice.primary.description" :theme="theme.description" :align="theme.align" :color="theme.color" />
													</div>
													<blockquote v-if="testimonial.quote" class="ps-testimonials__quote">
														<p>
																{{ $prismic.asText(testimonial.quote) }}
														</p>
														<cite class="ps-testimonials__cite">
															<prismic-image v-if="testimonial.profile_pic" v-bind="theme.image" class="ps-testimonials__author-img" :field="testimonial.profile_pic" />
															<div class="ps-testimonials__author">
																<p v-if="testimonial.name" class="ps-testimonials__author-name">{{ $prismic.asText(testimonial.name) }}</p>
																<p v-if="testimonial.role_position" class="ps-testimonials__author-title">{{ $prismic.asText(testimonial.role_position) }}</p>
															</div>
														</cite>
													</blockquote>
												</div>

                        <div class="ps-testimonials__img-wrapper span-7-12">
                          <prismic-image v-if="slice.primary.feature_illustration" class="ps-testimonials__img" :field="slice.primary.feature_illustration" />
                        </div>
                    </section>
                </div>

            </div>
        </div>
    </div>
	</ps-section>
</template>
<script>
import {
	PsEyebrow,
	PsDescription,
	PsTitle,
	PsSection,
} from '../../components'
import { commonProps } from '../../utils'

export default {
	components: {
		PsEyebrow,
		PsDescription,
		PsTitle,
		PsSection,
	},
	data() {
		return {
			index: 0,
		}
	},
	props: commonProps
}
</script>
<style lang="scss">
.ps-testimonials {
  .c-tabs {
    // this ensures the tablist is unerneath the tabpanel as per the design
    // no-flexbox browsers will show the tablist above the tabpanels by default
    // it is important to keep the tablist above the tabpanels in the source order for proper keyboard navigation
    display: flex;
    flex-direction: column-reverse;
  }

  .c-tabs__tablist {
    grid-row: 2;
    padding: var(--v-margin);


    @media all and (min-width: 50em) {
      margin-top: calc(var(--v-space) / 2);
    }
  }

  .c-tabs__tablist--imgs {
    margin-left: calc(var(--v-margin) * -1);
  }

  .c-tabs__tablist--imgs .c-tabs__tab {
    min-width: 44px;
		max-width: 22%;
    margin-right: 2rem;
    padding-bottom: .5rem;
    border-bottom: 4px solid transparent;

    &:last-of-type() {
      margin-right: 0;
    }

    &[aria-selected="true"] {
      border-bottom: 4px solid var(--color--primary);
    }

    &:active,
    &:focus,
    &:hover,
    &.focus-visible {
      outline: none;
      border-bottom: 4px solid #000;
    }

		img {
			width: 100%;
		}
  }

  .c-tabs__tabpanel {
    @media all and (min-width:50em) {
      display: flex;

      @supports (display:grid) {
        display: grid;
        grid-template-columns: repeat(12, 1fr);
        grid-column-gap: var(--h-padding);
        align-items: stretch;
      }

      &[hidden] {
        display: none; // we don't want the tabpanel to show up when it should be hidden
      }
    }
  }
}

.ps-testimonials__img-wrapper {
  background-color: var(--color--secondary);
  border-radius: 16px;


  @media all and (min-width:50em) {
    width: 100%;
    height: auto;
    margin-left: 2rem;

    @supports (display:grid) {
      width: auto;
      margin-left: 0;
    }
  }
}

.ps-testimonials__quote {
  background-color: #000;
  color: #fff;
  margin: var(--v-margin) 1rem var(--v-margin) 0;
  padding: 1.5rem;
  border-radius: 8px;
  position: relative;
  grid-row: 2;
  box-shadow: 0 12px 18px -8px rgba(0, 0, 0, 0.15), 0 12px 55px 10px rgba(0, 0, 0, 0.07);

  @media all and (min-width: 50em) {
    margin: 0 1rem 0 0;
  }

  font-size: calc(1rem * var(--text-min-l));

  * {
    line-height: 1.5;
  }

  @media screen and (min-width: 40rem) {
    font-size: calc(calc(1rem * var(--text-min-l)) + (var(--text-max-l) - var(--text-min-l)) * (100vw - 40rem) / (80 - 40));
  }

  @media screen and (min-width: 80rem) {
    font-size: calc(1rem * var(--text-max-l));
  }

  /* the triangle */
  &::after {
    display: block;
    position: absolute;
    content: "";
    width: 1rem;
    height: 1.625rem;
    background-image: url('./quote-triangle.svg');
    background-size: 100% auto;
    transform: rotate(90deg);
    background-repeat: no-repeat;
    bottom: -1rem;
    left: 2.5rem;

    @media all and (min-width: 50em) {
      transform: none;
      bottom: auto;
      left: auto;
      right: -1rem;
      top: 2.5rem;
    }
  }
}

.ps-testimonials__cite {
  font-size: 1rem;
  display: grid;
  grid-template-columns: auto 1fr;
  align-items: center;
  font-style: normal;
}

.ps-testimonials__author-img {
  width: 2rem;
  height: 2rem;
  margin-right: 1rem;
}

.ps-testimonials__author {
  p {
    margin-bottom: 0;
  }
}

.ps-testimonials__author-name {
  font-weight: bold;
}
</style>