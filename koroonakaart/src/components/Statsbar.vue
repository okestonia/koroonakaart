<template>
  <b-container id="statsbar-container" class="mb-5" fluid>
    <b-row><small style="margin-bottom: 20px">{{ $t("faq.a5") }}</small></b-row>
    <b-row>
      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("confirmedCases") }}</h5>
        </div>
        <h1>{{ confirmedCasesNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawConfirmedChanged === 0
              ? 'neutral'
              : rawConfirmedChanged > 0
              ? 'positive'
              : 'positive'">
          {{ confirmedChanged | formatNumber(currentLocale) }}
        </h5>
      </b-col>

      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("activeCases") }}</h5>
        </div>
        <h1>{{ activeCasesNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawActiveChanged === 0
              ? 'neutral'
              : rawActiveChanged > 0
              ? 'positive'
              : 'negative'">
          {{ activeChanged }}
        </h5>
      </b-col>

      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("perHundred") }}</h5>
        </div>
        <h1>{{ perHundred | formatNumber(currentLocale, 1) }}</h1>
        <h5 :class="
            rawPerHundredChanged === 0
              ? 'neutral'
              : rawPerHundredChanged > 0
              ? 'positive'
              : 'negative'">
          {{ rawPerHundredChanged | formatNumber(currentLocale, 1) }}
        </h5>
      </b-col>

      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("testsAdministered") }}</h5>
        </div>
        <h1>{{ testsAdministeredNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawTestsChanged === 0
              ? 'neutral'
              : rawTestsChanged > 0
              ? 'negative'
              : 'positive'">
          {{ testsChanged | formatNumber(currentLocale) }}
        </h5>
      </b-col>
    </b-row>
    <br>
    <b-row>
      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("hospitalised") }}</h5>
        </div>
        <h1>{{ hospitalisedNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawHospitalisedChanged === 0
              ? 'neutral'
              : rawHospitalisedChanged > 0
              ? 'positive'
              : 'negative'">
          {{ hospitalisedChanged | formatNumber(currentLocale) }}
        </h5>
      </b-col>

      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("recovered") }}</h5>
        </div>
        <h1>{{ recoveredNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawRecoveredChanged === 0
              ? 'neutral'
              : rawRecoveredChanged > 0
              ? 'negative'
              : 'positive'">
          {{ recoveredChanged | formatNumber(currentLocale) }}
        </h5>
      </b-col>

      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("deceased") }}</h5>
        </div>
        <h1>{{ deceasedNumber | formatNumber(currentLocale) }}</h1>
        <h5 :class="
            rawDeceasedChanged === 0
              ? 'neutral'
              : rawDeceasedChanged > 0
              ? 'positive'
              : 'negative'">
          {{ deceasedChanged | formatNumber(currentLocale) }}
        </h5>
      </b-col>
      <b-col class="statsbar-item" md>
        <div class="statsbar-heading">
          <h5>{{ $t("pos14avg") }}</h5>
        </div>
        <h1>{{ positiveTestAverage14Percent | formatNumber(currentLocale, 1) }}%</h1>
      </b-col>
    </b-row>
    <br>
    <b-row class="mb-2">
      <b-col md>
        <h5 class="text-left">{{ $t("vaccination") }}</h5>
      </b-col>
    </b-row>
    <b-row>
      <b-col class="statsbar-item" md>
        <div class="vaccination-progress-label">
          <p class="vaccination-progress-text">{{ $t("atLeastOneDose") }}</p>
          <p class="vaccination-progress-value">{{ vaccinatedAtLeastOneDosePercentage | formatNumber(currentLocale, 1) }}%</p>
        </div>
        <div class="progress">
          <div class="progress-bar"
               :style="{width: vaccinatedAtLeastOneDosePercentage + '%'}">
          </div>
        </div>
      </b-col>
      <b-col class="statsbar-item" md>
        <div class="vaccination-progress-label">
          <p class="vaccination-progress-text">{{ $t("fullyVaccinated") }}</p>
          <p class="vaccination-progress-value">{{ fullyVaccinatedNumberPercentage | formatNumber(currentLocale, 1) }}%</p>
        </div>
        <div class="progress">
          <div class="progress-bar"
               :style="{width: fullyVaccinatedNumberPercentage + '%'}">
          </div>
        </div>
      </b-col>
    </b-row>


  </b-container>
</template>


<script>
import data from "../data.json";
import { positiveSign } from "../utilities/helper";
import { formatNumberByLocale } from "../utilities/formatNumberByLocale";
export default {
  name: "Statsbar",

  data() {
    return {
      activeCasesNumber: data.activeCasesNumber,
      positiveTestAverage14Percent:
        data.dataTestsPerDayChart.positiveTestAverage14Percent,
      confirmedCasesNumber: data.confirmedCasesNumber,
      deceasedNumber: data.deceasedNumber,
      perHundred:
        data.dataCumulativeCasesChart.active100k[
          data.dataCumulativeCasesChart.active100k.length - 1
        ],
      hospitalisedNumber:
        data.hospital.activehospitalizations[
          data.hospital.activehospitalizations.length - 1
        ],
      recoveredNumber:
        data.hospital.discharged[data.hospital.discharged.length - 1],
      testsAdministeredNumber: data.testsAdministeredNumber,

      rawActiveChanged: Number(
        data.dataCumulativeCasesChart.active[
          data.dataCumulativeCasesChart.active.length - 1
        ] -
          data.dataCumulativeCasesChart.active[
            data.dataCumulativeCasesChart.active.length - 2
          ]
      ),
      rawConfirmedChanged: Number(
        data.dataNewCasesPerDayChart.confirmedCases[
          data.dataNewCasesPerDayChart.confirmedCases.length - 1
        ]
      ),
      rawPerHundredChanged: positiveSign(
        Number(
          data.dataCumulativeCasesChart.active100k[
            data.dataCumulativeCasesChart.active100k.length - 1
          ] -
            data.dataCumulativeCasesChart.active100k[
              data.dataCumulativeCasesChart.active100k.length - 2
            ]
        ).toFixed(2)
      ),
      rawDeceasedChanged: Number(data.deceasedChanged),
      rawHospitalisedChanged: Number(data.hospitalChanged),
      rawRecoveredChanged: Number(data.recoveredChanged),
      rawTestsChanged: Number(
        data.dataCumulativeTestsChart.testsAdministered[
          data.dataCumulativeTestsChart.testsAdministered.length - 1
        ] -
          data.dataCumulativeTestsChart.testsAdministered[
            data.dataCumulativeTestsChart.testsAdministered.length - 2
          ]
      ),

      activeChanged: positiveSign(data.activeChanged),
      deceasedChanged: positiveSign(data.deceasedChanged),
      confirmedChanged: positiveSign(
        data.dataNewCasesPerDayChart.confirmedCases[
          data.dataNewCasesPerDayChart.confirmedCases.length - 1
        ]
      ),
      hospitalisedChanged: positiveSign(data.hospitalChanged),
      recoveredChanged: positiveSign(data.recoveredChanged),
      testsChanged: positiveSign(
        data.dataCumulativeTestsChart.testsAdministered[
          data.dataCumulativeTestsChart.testsAdministered.length - 1
        ] -
          data.dataCumulativeTestsChart.testsAdministered[
            data.dataCumulativeTestsChart.testsAdministered.length - 2
          ]
      ),
      vaccinatedAtLeastOneDoseNumber: data.vaccinatedAtLeastOneDoseNumber,
      vaccinatedAtLeastOneDoseChange: positiveSign(data.vaccinatedAtLeastOneDoseChange),
      vaccinatedAtLeastOneDosePercentage: data.vaccinatedAtLeastOneDosePercentage,
      fullyVaccinatedNumber: data.fullyVaccinatedNumber,
      fullyVaccinatedNumberChange: positiveSign(data.fullyVaccinatedNumberChange),
      fullyVaccinatedNumberPercentage: data.fullyVaccinatedNumberPercentage,
    };
  },

  computed: {
    currentLocale: function () {
      return this.$i18n.locale;
    },
  },

  filters: {
    formatNumber: function (number, currentLocale, precision) {
      let result = "";

      if (typeof number === "string" && number.startsWith("+")) {
        const actualNumber = number.split("+")[1];
        result = "+" + formatNumberByLocale(actualNumber, currentLocale, precision);
      } else {
        result = formatNumberByLocale(number, currentLocale, precision);
      }

      return result.replace(/\s/g, "\u202F");
    },
  },
};
</script>


<style lang="scss" scoped>
h5 {
  font-size: 18px;
  font-weight: 500;
}

.vaccination-progress-label {
  display: flex;
  justify-content: space-between;
}
.vaccination-progress-text {
  font-size: 16px;
  text-align: left;
  margin-bottom: 0.25rem;
}
.vaccination-progress-value {
  font-size: 16px;
  margin-bottom: 0.25rem;
}

#statsbar-container {
  @media only screen and (max-width: 766px) {
    margin-bottom: 0;
  }
}

.statsbar-heading {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 4em;
  margin-bottom: -0.4em;
}

.positive {
  color: red;
}

.negative {
  color: green;
}

.neutral {
  color: rgb(97, 97, 97);
}

.progress {
  margin-bottom: 1rem;
}

.progress-bar {
  background-color: #7cb5ec;
}

.statsbar-item {
  //  height: 7em;

  @media only screen and (max-width: 766px) {
    margin-bottom: 1em;
  }
}
</style>
