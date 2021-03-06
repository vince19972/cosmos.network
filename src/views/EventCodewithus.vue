<template lang="pug">
div
  tm-section.component__hero__container(theme="dark")
    div(slot="suptitle").hero-suptitle May - Dec 2020 · Online series
    div(slot="title").hero-title Code with Us
    div(slot="subtitle").hero-subtitle Join us in a live, interactive workshop program designed to educate and provide real-time practice to developers in the Cosmos community.
    tm-btn(size="lg" value="sign up now" color="primary" @click.native="openModal(true)")
    .next-workshop(v-for="i in nextWorkshop")
      .next-workshop__display Next workshop
      .next-workshop__title {{ i.title }}
      .next-workshop__date {{ moment(i.date).format("MMM DD") }}

  tm-section(layout="center").center-section
    div(slot="title").center-title Jump start your Cosmos coding journey
    div(slot="subtitle").center-subtitle The #[i Code with Us] program aims to provide everyone with access to the information and resources they need to become familiar with the Cosmos stack. #[br] #[br] These seminars are small workshops led by expert mentors from Tendermint Inc, Interchain GmbH, Agoric, Band Protocol and others that will use their expertise to guide the community of developers from deploying a simple “Hello World” blockchain, to getting familiar with the Cosmos SDK architecture, running an IBC relayer, and much more. We have put together a list of future workshops designed to educate, guide and inspire the next generation of innovators.

  tm-section
    div(slot="title") Hosts
    .hosts-container
      .item(v-for="i in hosts")
        img(:src="`/profiles/${i.id}.jpg`").item__img
        .profile
          .profile__name {{ i.name }}
          .profile__company {{ i.company }}
          .profile__social(v-if="i.twitter")
            a(:href="`https://twitter.com/${i.twitter}`" target="_blank" rel="noreferrer noopener") @{{ i.twitter }}
          .profile__social(v-else-if="i.social")
            a(:href="`${i.social.url}`" target="_blank" rel="noreferrer noopener") {{ i.social.title }}
    .note More hosts to be announced.

  tm-section
    div(slot="title") Workshops
    .schedule-container
      .row(v-for="i in workshops")
        .row-start
          .row-start__date {{ moment(i.date).format("MMM DD")}}
          .row-start__time(v-if="moment(i.date) > moment() || !i.replay")  {{ toTimezone(i.date, i.time) }}
          .row-start__time(v-else)
            a(:href="`${i.replay}`" target="_blank" rel="noreferrer noopener") Watch replay
        .row-mid
          .row-mid__title #[router-link(:to="{ name: 'series-workshop', params: { workshop: i.slug }}") {{ i.title }}]
        .row-end
          img(:src="`/profiles/${i.id}.jpg`" v-if="i.id").row-end__img
          div(v-else-if="i.ids")
            img(:src="`/profiles/${i.ids[0]}.jpg`" ).row-end__img
            img(:src="`/profiles/${i.ids[1]}.jpg`" ).row-end__img__overlay
          .row-end__profile
            .row-end__profile__host {{ i.host }}
            .row-end__profile__company {{ i.company }}
    .future-workshops More workshops to be announced
    .unconfirmed-note(style="max-width: 44rem;") * Unconfirmed (TBD) dates and times will be decided based on the timezone of participants. Let us know where you are based when you sign up.

  tm-section(layout="center")
    div(slot="title").center-title How to join
    .steps-container
      .item
        .item__num 1
        .item__title Sign up
        .item__desc Check your inbox for a confirmation email to confirm your registration.
        .item__cta(@click="openModal(true)") sign up now
      .item
        .item__num 2
        .item__title Add the calendar
        .item__desc We'll send you a calendar link and reminders before each workshop.
      .item
        .item__num 3
        .item__title Join the call
        .item__desc Make sure to install the #[a(href="https://zoom.us/download" target="_blank" rel="noreferrer noopener") Zoom] app beforehand and join the Cosmos community chat on Discord.
        a(href="https://discord.gg/W8trcGV" target="_blank" rel="noreferrer noopener").item__cta community chat

  tm-section
    div(slot="title") FAQ
    .faq-container
      .item(v-for="i in questions")
        .item__question {{ i.question }}
        .item__answer(v-html="markdown(i.answer)")

  tm-section
    div(slot="title") Previous workshops
    .tm-cards-hz--horizontal-scrolling
      tm-card-hz(
        v-for="i in pastWorkshops"
        flush="true"
        :key="i.id"
        :title="`Code with us - ${i.title} with ${i.host}`"
        :img-src="i.coverImg"
        :href="i.replay")
        template(slot="subtitle") {{ moment(i.date).format("MMM DD, YYYY") }}

  .sign-up-section.component__hero__container
    .left
      .text Sign up to join the next workshop
    .right
      tm-btn(size="lg" value="sign up" color="primary" @click.native="openModal(true)")

  modal(v-bind="{visible: !!(signup), side: 'center', buttonClose: true, fullscreen: true, width: '44rem', backgroundColor: 'rgba(0,0,0,.8)'}" @visible="signup = $event" v-if="!!(signup)")
    .modal__content(v-if="!!(signup)")
      series-signup
        template(slot="h1") Sign up for Code with Us
</template>

<script>
import moment from "moment-timezone"
import MarkdownIt from "markdown-it"
import { mapGetters } from "vuex"
import { Modal, SeriesSignup } from "@cosmos-ui/vue"
import TmSection from "common/TmSection"
import TmBtn from "common/TmBtn"
import TmCardHz from "cards/TmCardHz"

export default {
  name: "page-event-code-with-us",
  metaInfo: { title: "Code with Us" },
  components: {
    Modal,
    SeriesSignup,
    TmSection,
    TmBtn,
    TmCardHz
  },
  data() {
    return {
      moment: moment,
      signup: null,
      hosts: [
        {
          id: "host-alessio",
          name: "Alessio Treglia",
          company: "Cosmos | Tendermint Inc",
          twitter: "alessiotreglia"
        },
        {
          id: "host-billy",
          name: "Billy Rennekamp",
          company: "Interchain GmbH",
          twitter: "billyrennekamp"
        },
        {
          id: "host-dean",
          name: "Dean Tribble",
          company: "Agoric",
          twitter: "DeanTribble"
        },
        {
          id: "host-elizabeth",
          name: "Elizabeth Binks",
          company: "Chainsafe",
          social: {
            title: "GitHub",
            url: "https://github.com/noot"
          }
        },
        {
          id: "host-ethan",
          name: "Ethan Frey",
          company: "Confio | CosmWasm",
          social: {
            title: "LinkedIn",
            url: "https://linkedin.com/in/ethanfrey"
          }
        },
        {
          id: "host-marko",
          name: "Marko Baricevic",
          company: "Interchain GmbH",
          social: {
            title: "GitHub",
            url: "https://github.com/marbar3778"
          }
        },
        {
          id: "host-sai",
          name: "Sai Krishna Sunkari",
          company: "Cosmic Compass",
          twitter: "Saisunkari19"
        },
        {
          id: "host-sistla",
          name: "Sistla V Abhishek",
          company: "Cosmic Compass",
          twitter: "svabhishek"
        },
        {
          id: "host-sorawit",
          name: "Sorawit Suriyakarn",
          company: "Band Protocol",
          twitter: "nomorebear"
        },
        {
          id: "host-sunny",
          name: "Sunny Aggarwal",
          company: "Cosmos | Tendermint Inc",
          twitter: "sunnya97"
        },
        {
          id: "host-valery",
          name: "Valery Litvin",
          company: "Cyber_Devs",
          twitter: "litvintech"
        },
        {
          id: "host-denis",
          name: "Denis Fadeev",
          company: "Tendermint",
          twitter: "fadeev"
        }
      ],
      questions: [
        {
          question: "How do I join the call?",
          answer:
            "You'll need to download [Zoom](https://zoom.us/download) beforehand. Once you've signed up, we'll send out a link to join the call before each workshop."
        },
        {
          question: "Can I share a project or tutorial?",
          answer:
            "Yes, please do! Join the [Cosmos Community chat](https://discord.gg/W8trcGV) to share any relevant projects or tutorials."
        },
        {
          question: "Can I host my own workshop?",
          answer:
            "Yes, you can! Contact [Cosmos marketing team](mailto:communications@cosmos.network) with the details of your proposed workshop. The marketing team will review your workshop proposal and get back to you as soon as possible."
        },
        {
          question: "Still have questions?",
          answer:
            "Reach out on the [community chat](https://discord.gg/W8trcGV) (#code-with-us-workshops channel) for assistance."
        }
      ]
    }
  },
  computed: {
    ...mapGetters(["workshops"]),
    nextWorkshop() {
      const workshop = this.workshops
        .filter(e => moment(e.date) >= moment())
        .slice(0, 1)
      return workshop
    },
    pastWorkshops() {
      const workshop = this.workshops
        .filter(e => moment(e.date) <= moment())
        .filter(e => Boolean(e.replay))
      return workshop
    }
  },
  methods: {
    toTimezone(date, time) {
      return (
        moment
          // set base time with PDT
          // get timezone with i18n API - Intl.DateTimeFormat().resolvedOptions().timeZone
          // usage: 2020-08-04 08:00
          .tz(`${date} ${time}`, "America/Los_Angeles")
          // use client's local time zone
          .tz(moment.tz.guess())
          .format("h:mma z")
      )
    },
    markdown(value) {
      const md = new MarkdownIt({
        linkify: true,
        html: true
      })
      return md.render(value)
    },
    openModal(bool) {
      this.$gtm.trackEvent({
        event: "event",
        category: "Code with Us",
        action: "click",
        label: "Code with Us Signup Click",
        value: true
      })
      this.signup = bool
    }
  }
}
</script>

<style lang="stylus" scoped>
a
  color #5064FB
  text-decoration none
  cursor pointer

.center-section
  max-width 64rem
  margin auto

.center-title
  text-align left

.center-subtitle
  text-align left
  color rgba(0, 0, 0, 0.8)
  margin-top 3rem

.component__hero__container
  background-image url('~assets/images/home/section-codewithus.jpg')
  background-size cover
  background-repeat no-repeat
  background-position center center
  &:first-child
    margin-top 4rem
  .hero-title
    font-weight var(--fw-semibold)
    font-size 3rem
    line-height 3.75rem
    letter-spacing var(--tracking-1-tight)
  .hero-suptitle
    text-transform uppercase
    font-weight var(--fw-bold)
    font-size 1rem
    line-height 1.25rem
    letter-spacing var(--tracking-2-wide)
    color rgba(255, 255, 255, 0.75)
  .hero-subtitle
    max-width 44rem

.next-workshop
  margin-top 7rem
  &__display
    font-weight var(--fw-bold)
    font-size 1rem
    line-height 1.25rem
    letter-spacing var(--tracking-2-wide)
    text-transform uppercase
    color #BFBFBF
  &__title
    margin-top 0.5rem
    font-weight var(--fw-bold)
    font-size 1.125rem
    line-height 1.6875rem
    display flex
    align-items center
    letter-spacing var(--tracking-1-wide)
    color #FFFFFF
  &__date
    margin-top 0.5rem
    font-size 0.875rem
    line-height 1.25rem
    display flex
    align-items center
    letter-spacing var(--tracking-1-wide)
    color #BFBFBF

.hosts-container
  margin auto
  display grid
  grid-template-columns repeat(auto-fit, minmax(373px, 1fr))
  gap 3rem
  margin-top 4rem
  .item
    &__img
      float left
      width 6.75rem
      height 6.75rem
      border-radius 50%
      display flex
      justify-content center
      align-items center
  .profile
    padding-left 1rem
    display flex
    flex-direction column
    text-align start
    padding-top 1rem
    padding-bottom 1rem
    &__name
      font-weight var(--fw-bold)
      font-size 1.125rem
      line-height 1.6875rem
      letter-spacing var(--tracking-1-wide)
      color #000000
    &__company
      font-size 1rem
      line-height 1.5rem
      color rgba(0, 0, 0, 0.667)
    &__social
      font-size 1rem
      line-height 1.5rem
      color #5064FB
.note
  margin-top 4.5rem
  display flex
  font-size 1.125rem
  line-height 1.6875rem
  display flex
  align-items center
  letter-spacing var(--tracking-0)
  color rgba(0, 0, 0, 0.667)

.unconfirmed-note
  margin-top 4.5rem
  display flex
  font-size 0.875rem
  line-height 1.25rem
  display flex
  align-items center
  letter-spacing var(--tracking-0)
  color rgba(0, 0, 0, 0.667)

.schedule-container
  margin-top 4rem
  .row
    display grid
    grid-template-columns 30% 50% 20%
    margin auto
    padding-top 2.9063rem
    padding-bottom 2.9063rem
    border-bottom 1px solid rgba(59,66,125,0.12)
    &:first-child
      border-top 1px solid rgba(59,66,125,0.12)
    .row-start
      &__date
        font-weight var(--fw-bold)
        font-size 1rem
        line-height 1.25rem
        letter-spacing var(--tracking-2-wide)
        text-transform uppercase
        color #000000
      &__time
        margin-top 0.25rem
        font-size 0.875rem
        line-height 1.25rem
        letter-spacing var(--tracking-1-wide)
        color rgba(0, 0, 0, 0.667)
    .row-mid
      align-self center
      &__title
        font-weight var(--fw-semibold)
        font-size 1.5rem
        line-height 2rem
        letter-spacing var(--tracking-1-wide)
        color #000000
    .row-end
      align-self center
      &__img
        float left
        width 3rem
        height 3rem
        border-radius 50%
        display flex
        justify-content center
        align-items center
        &__overlay
          float left
          width 3rem
          height 3rem
          border-radius 50%
          display flex
          justify-content center
          align-items center
          position relative
          left -5px
      &__profile
        display flex
        flex-direction column
        text-align start
        padding-left 1rem
        &__host
          font-weight var(--fw-bold)
          font-size 1rem
          line-height 1.5rem
          color #000000
        &__company
          margin-top 0.25rem
          font-size 0.875rem
          line-height 1.25rem
          letter-spacing var(--tracking-1-wide)
          color rgba(0, 0, 0, 0.667)

.future-workshops
  display flex
  justify-content center
  align-items center
  background rgba(176, 180, 207, 0.087)
  padding 3rem

.steps-container
  margin-top 4rem
  display grid
  grid-template-columns repeat(auto-fit, minmax(320px, 1fr))
  gap 2rem
  .item
    margin-top 2rem
    &__num
      display flex
      justify-content center
      align-items center
      background linear-gradient(149.47deg, rgba(65, 84, 224, 0.910609) 47.91%, #ABB3ED 113.73%)
      border-radius 2.25rem
      width 4.5rem
      height 4.5rem
      margin auto
      font-size 2rem
      font-weight var(--fw-regular)
      line-height 2.5rem
      letter-spacing var(--tracking-1-wide)
      color #FFFFFF
    &__title
      margin-top 1rem
      font-weight var(--fw-semibold)
      font-size 1.5rem
      line-height 2rem
      text-align center
      letter-spacing var(--tracking-1-wide)
      color #0B0C17
    &__desc
      margin-top 1rem
      font-size 1.125rem
      line-height 1.6875rem
      margin-bottom 2rem
    &__cta
      color #5064FB
      font-weight var(--fw-bold)
      font-size 1rem
      line-height 1.5rem
      text-transform uppercase
      cursor pointer

.faq-container
  margin-top 4rem
  display grid
  grid-template-columns repeat(auto-fit, minmax(320px, 1fr))
  gap 2rem
  .item
    padding-top 2rem
    &__question
      font-weight var(--fw-bold)
      font-size 1.125rem
      line-height 1.6875rem
    &__answer
      margin-top 1rem
      font-size 1.125rem
      line-height 1.6875rem
      letter-spacing var(--tracking-0)
    &:nth-child(4)
      background #F8F9FC
      border 2px solid rgba(59, 66, 125, 0.12)
      box-sizing border-box
      border-radius 0.25rem
      padding 2rem

.tm-cards-hz--horizontal-scrolling
  margin-top 4rem

.sign-up-section
  display grid
  grid-template-columns 50% 20%
  grid-template-rows 1fr
  justify-content space-evenly
  padding 5.75rem 8rem
  .left
    display flex
    align-items center
    .text
      font-weight var(--fw-semibold)
      font-size 2rem
      line-height 2.5rem
      letter-spacing var(--tracking-1-tight)
      color #FFFFFF
  .right
    display grid
    grid-auto-flow column
    grid-template-columns 1fr min-content
    gap 1rem
    &__input
      &__input
        outline none
        width 100%
        height 4rem
        background rgba(255,255,255,.1)
        border none
        border-radius 0.25rem
        color white
        font-size 1rem
        padding 0.75rem 1rem
        line-height var(--lh-paragraph)
        box-sizing border-box
        transition all .15s
        backdrop-filter blur(1.5rem)
        font-weight var(--fw-regular)
        margin 0

@media screen and (max-width: 1023px)
  .center-title
    margin-top 0rem
    text-align start
  .sign-up-section
    grid-template-columns auto
    padding 5.75rem 2rem
    justify-content center
    .left
      display block
      .text
        padding-bottom 2rem
    .right
      &__input__input
        height 3.75rem

@media screen and (max-width: 1024px)
  .schedule-container
    .row
      display block

@media screen and (max-width: 900px)
  .hosts-container
    display table
    .item
      display flex
      margin 2rem auto

@media screen and (max-width: 700px)
  .note
    text-align center
  .hosts-container, .schedule-container
    margin-top 2rem
  .schedule-container
    .row
      .row-start, .row-start__time, .row-mid, .row-end
        margin 0.5rem auto
  .steps-container, .faq-container
    padding-top 2rem
    gap 4rem
  .faq-container
    .item
      padding-top 0

@media screen and (max-width: 320px)
  .component__hero__container
    .hero-title
      font-size 2.5rem
  .steps-container, .faq-container
    grid-template-columns repeat(auto-fit, minmax(250px, 1fr))
</style>
