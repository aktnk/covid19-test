<template>
  <div>
    <confirmed-cases-details-card
      v-if="this.$route.params.card == 'details-of-confirmed-cases'"
    />
    <confirmed-cases-number-card
      v-else-if="this.$route.params.card == 'number-of-confirmed-cases'"
    />
    <confirmed-cases-attributes-card
      v-else-if="this.$route.params.card == 'attributes-of-confirmed-cases'"
    />
    <tested-number-card
      v-else-if="this.$route.params.card == 'number-of-tested'"
    />
    <consultation-desk-reports-number-card
      v-else-if="
        this.$route.params.card ==
          'number-of-reports-to-covid19-consultation-desk'
      "
    />
  </div>
</template>

<script>
import Data from '@/data/data.json'
import ConfirmedCasesDetailsCard from '@/components/cards/ConfirmedCasesDetailsCard.vue'
import ConfirmedCasesNumberCard from '@/components/cards/ConfirmedCasesNumberCard.vue'
import ConfirmedCasesAttributesCard from '@/components/cards/ConfirmedCasesAttributesCard.vue'
import TestedNumberCard from '@/components/cards/TestedNumberCard.vue'
import ConsultationDeskReportsNumberCard from '@/components/cards/ConsultationDeskReportsNumberCard.vue'

export default {
  components: {
    ConfirmedCasesDetailsCard,
    ConfirmedCasesNumberCard,
    ConfirmedCasesAttributesCard,
    TestedNumberCard,
    ConsultationDeskReportsNumberCard
  },
  data() {
    let title, updatedAt
    switch (this.$route.params.card) {
      case 'details-of-confirmed-cases':
        title = this.$t('検査陽性者の状況')
        updatedAt = Data.patients.date
        break
      case 'number-of-confirmed-cases':
        title = this.$t('陽性患者数')
        updatedAt = Data.patients_summary.date
        break
      case 'attributes-of-confirmed-cases':
        title = this.$t('陽性患者の属性')
        updatedAt = Data.patients.date
        break
      case 'number-of-tested':
        title = this.$t('検査実施件数')
        updatedAt = Data.inspections_summary.date
        break
      case 'number-of-reports-to-covid19-consultation-desk':
        title = this.$t('新型コロナ受診相談窓口相談件数')
        updatedAt = Data.querents.date
        break
    }

    const data = {
      title,
      updatedAt
    }
    return data
  },
  head() {
    const url = 'https://stopcovid19.code4numazu.org/'
    const timestamp = new Date().getTime()
    const ogpImage =
      this.$i18n.locale === 'ja'
        ? `${url}/ogp/${this.$route.params.card}.png?t=${timestamp}`
        : `${url}/ogp/${this.$i18n.locale}/${this.$route.params.card}.png?t=${timestamp}`
    const description = `${this.updatedAt} | ${this.$t(
      '当サイトは新型コロナウイルス感染症 (COVID-19) に関する最新情報を提供するために、静岡県とCode for ふじのくにが開設したものです。'
    )}`

    return {
      title: this.title,
      meta: [
        {
          hid: 'og:url',
          property: 'og:url',
          content: url + this.$route.path + '/'
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content:
            this.title +
            ' | ' +
            this.$t('静岡県') +
            ' ' +
            this.$t('新型コロナウイルス感染症') +
            this.$t('対策サイト')
        },
        {
          hid: 'description',
          name: 'description',
          content: description
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: description
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: ogpImage
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: ogpImage
        }
      ]
    }
  }
}
</script>
