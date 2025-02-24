<template>
  <div class="syllabus">
    <!-- TODO: update section header description and provide translation -->
    <AccountMenu
      :first-name="firstName"
      :last-name="lastName"
      :active-menu-item="syllabusParentPath"
      :section-list="sectionList"
    />
    <div class="syllabus__container">
      <UserAccountSectionHeader>
        <template #title>
          {{ $translate("Syllabus") }}
        </template>
        <template #description>
          {{ $translate("This is placeholder text for a page where students are viewing a syllabus.") }}
        </template>
      </UserAccountSectionHeader>
      <SyllabusView :syllabus="syllabus" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue-demi'
import { getActiveSyllabus, Syllabus } from '../../../ts/syllabus'
import { ACCOUNT_MENU_LINKS } from '../constants/accountMenuLinks'
import UserAccountSectionHeader from '../UserAccount/UserAccountSectionHeader.vue'
import AccountMenu from '../UserAccount/AccountMenu.vue'
import SyllabusView from './SyllabusView.vue'

export default defineComponent({
  name: 'SyllabusLayout',
  components: {
    UserAccountSectionHeader,
    SyllabusView,
    AccountMenu
  },
  props: {
    firstName: { type: String, required: false, default: '' },
    lastName: { type: String, required: false, default: '' }
  },
  data () {
    return {
      syllabus: undefined as Syllabus | undefined,
      userIsLoggedIn: false,
      sectionList: ACCOUNT_MENU_LINKS,
      syllabusParentPath: '/account/classroom'
    }
  },
  mounted () {
    this.syllabus = getActiveSyllabus()
  }
})
</script>

<style lang="scss" scoped>
@import 'carbon-components/scss/globals/scss/typography';
@import '~/../scss/variables/mq.scss';
@import '~/../scss/mixins/mixins.scss';

.syllabus {
  display: grid;
  grid-template-columns: 17rem 1fr;
  height: calc(100vh - 60px);

  @include mq($from: medium, $until: large) {
    grid-template-columns: 12rem 1fr;
  }

  @include mq($until: medium) {
    grid-template-columns: 1fr;
    grid-template-rows: min-content 1fr;
  }

  &__container {
    @include contained();
    margin-left: 0;
    padding: $spacing-07 $spacing-08;
  }

  &__not-found {
    @include type-style('expressive-heading-04');
    padding-top: $spacing-07;
  }
}
</style>
