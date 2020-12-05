<template>
  <form class="form" @submit.prevent="checkValid">
    <div class="form__decoration-right"></div>
    <div class="form__decoration-left"></div>
    <div class="form__decoration-circle"></div>
    <div>
      <section class="form__grid">
        <h2 class="from__header from__header--general">Общая информация</h2>
        <div class="from__item">
          <label for="lastName">Фамилия</label>
          <input
            class="from__control"
            :class="$v.form.lastName.$error ? 'from__control--invalid' : ''"
            id="lastName"
            type="text"
            placeholder="Иванов"
            v-model="form.lastName"
          />
          <p v-if="$v.form.lastName.$dirty && !$v.form.lastName.required" class="form__required">
            Обязательное поле
          </p>
        </div>
        <div class="from__item">
          <label for="firstName">Имя</label>
          <input
            class="from__control"
            :class="$v.form.firstName.$error ? 'from__control--invalid' : ''"
            id="firstName"
            type="text"
            placeholder="Иван"
            v-model="form.firstName"
          />
          <p v-if="$v.form.firstName.$dirty && !$v.form.firstName.required" class="form__required">
            Обязательное поле
          </p>
        </div>
        <div class="from__item">
          <label for="middleName">Отчество</label>
          <input
            class="from__control"
            id="middleName"
            type="text"
            placeholder="Иванович"
            v-model="form.middleName"
          />
        </div>
        <div class="from__item">
          <label for="birthDate">Дата рождения</label>
          <input
            class="from__control"
            :class="$v.form.birthDate.$error ? 'from__control--invalid' : ''"
            id="birthDate"
            type="date"
            v-model="form.birthDate"
          />
          <p v-if="$v.form.birthDate.$dirty && !$v.form.birthDate.required" class="form__required">
            Обязательное поле
          </p>
        </div>
        <div class="from__item">
          <p class="from__gender-text">Выберите пол</p>
          <input
            id="genderMale"
            class="from__radio visually-hidden"
            type="radio"
            name="gender"
            value="male"
            v-model="form.gender"
          />
          <label class="from__gender" for="genderMale">Мужской</label>
          <input
            id="genderFemale"
            class="from__radio visually-hidden"
            type="radio"
            name="gender"
            value="female"
            v-model="form.gender"
          />
          <label class="from__gender" for="genderFemale">Женский</label>
        </div>
        <div class="from__item">
          <label for="doctors">Лечащий врач:</label>
          <select id="doctors" v-model="form.selectedDoctor" class="from__control">
            <option
              v-for="(doctor, index) in doctors"
              :value="doctor"
              :key="index"
            >
              {{doctor}}
            </option>
          </select>
        </div>
        <div class="from__item">
          <label for="tel">Номер телефона</label>
          <input
            class="from__control"
            id="tel"
            type="number"
            :class="$v.form.tel.$error ? 'from__control--invalid' : ''"
            placeholder="79648006050"
            v-model="form.tel"
          />
          <p v-if="$v.form.tel.$dirty && !$v.form.tel.required" class="form__required">
            Обязательное поле
          </p>
          <p v-if="$v.form.tel.$dirty && !$v.form.tel.minLength" class="form__required">
            Минимальное кол-во цифры {{ 11 - $v.form.tel.$model.length }}
          </p>

          <div class="form__sms">
            <input
              class="form__sms-control visually-hidden"
              id="sms"
              type="checkbox"
              v-model="form.sms"
            >
            <label class="form__sms-label" for="sms">Не отправлять СМС</label>
          </div>
        </div>
        <div class="from__item">
          <label for="groupsPeople">Группа</label>
          <select id="groupsPeople" v-model="form.selectedGroup" class="from__control" :class="{ 'from__control--groups': isActiveGroups() }" multiple>
            <option
              v-for="(group, index) in groupsPeople"
              :value="group.value"
              :class="$v.form.selectedGroup.$error ? 'from__control--invalid' : ''"
              :key="index"
            >
              {{group.label}}
            </option>
          </select>
          <p v-if="$v.form.selectedGroup.$dirty && !$v.form.selectedGroup.required" class="form__required">
            Выберите группу(-ы)
          </p>
        </div>
      </section>
      <section class="form__grid">
        <h2 class="from__header">Адрес</h2>
        <div class="from__item">
          <label for="mailIndex">Индекс</label>
          <input
            id="mailIndex"
            placeholder="193421"
            class="from__control"
            type="number"
            min="0"
            v-model.trim="form.mailIndex"
          >
        </div>
        <div class="from__item">
          <label for="country">Страна</label>
          <input
            id="country"
            placeholder="Россия"
            class="from__control"
            type="text"
            v-model="form.country"
          >
        </div>
        <div class="from__item">
          <label for="city">Город</label>
          <input
            id="city"
            placeholder="Санкт-Петербург"
            class="from__control"
            type="text"
            :class="$v.form.city.$error ? 'from__control--invalid' : ''"
            v-model="form.city"
          >
          <p v-if="$v.form.city.$dirty && !$v.form.city.required" class="form__required">
            Обязательное поле
          </p>
        </div>
        <div class="from__item">
          <label for="street">Улица</label>
          <input
            id="street"
            placeholder="Партизана Германа"
            class="from__control"
            type="text"
            v-model="form.street"
          >
        </div>
        <div class="from__item">
          <label for="house">Дом</label>
          <input
            id="house"
            placeholder="15"
            class="from__control"
            type="text"
            v-model="form.house"
          >
        </div>
      </section>
      <section class="form__grid">
        <h2 class="from__header">{{form.selectedTypeDocs}}</h2>
        <div class="from__item">
          <label for="selectedTypeDocs">Тип документа</label>
          <select v-model="form.selectedTypeDocs" class="from__control" id="selectedTypeDocs">
            <option
              v-for="(typeDoc, index) in typeDocs"
              :key="index"
              :class="$v.form.selectedTypeDocs.$error ? 'from__control--invalid' : ''"
            >
            {{typeDoc}}
            </option>
          </select>
          <p v-if="$v.form.selectedTypeDocs.$dirty && !$v.form.selectedTypeDocs.required" class="form__required">
            Обязательное поле
          </p>
        </div>
        <div class="from__item">
          <label class="seriesDoc">Серия</label>
          <input
            id="seriesDoc"
            class="from__control"
            type="nubmer"
            placeholder="1415"
            v-model="form.seriesDoc"
          >
        </div>
        <div class="from__item">
          <label class="numberDocs">Номер</label>
          <input
            id="numberDocs"
            class="from__control"
            placeholder="141516"
            type="nubmer"
            v-model="form.numberDocs"
          >
        </div>
        <div class="from__item">
          <label for="givenDocs">Кем выдан</label>
          <input
            id="givenDocs"
            placeholder="ГУ МВД России..."
            class="from__control"
            type="text"
            v-model="form.givenDocs"
          >
        </div>
        <div class="from__item">
          <label for="dateGivenDocs">Дата выдачи</label>
          <input
            id="dateGivenDocs"
            class="from__control"
            type="date"
            :class="$v.form.dateGivenDocs.$error ? 'from__control--invalid' : ''"
            v-model="form.dateGivenDocs"
          >
          <p v-if="$v.form.dateGivenDocs.$dirty && !$v.form.dateGivenDocs.required" class="form__required">
            Обязательное поле
          </p>
        </div>
      </section>
      <button class="form__btn" type="submit">Отправить</button>
    </div>
    <transition name="bounce">
      <div class="form__popupSuccess" :class="{'form__popupSuccess--active': submitSuccsess}">
        <svg class="form__popupSuccess-svg" height="24" viewBox="0 0 512 512" width="24" xmlns="http://www.w3.org/2000/svg"><path d="m256 0c-141.164062 0-256 114.835938-256 256s114.835938 256 256 256 256-114.835938 256-256-114.835938-256-256-256zm0 0" fill="#2196f3"/><path d="m385.75 201.75-138.667969 138.664062c-4.160156 4.160157-9.621093 6.253907-15.082031 6.253907s-10.921875-2.09375-15.082031-6.253907l-69.332031-69.332031c-8.34375-8.339843-8.34375-21.824219 0-30.164062 8.339843-8.34375 21.820312-8.34375 30.164062 0l54.25 54.25 123.585938-123.582031c8.339843-8.34375 21.820312-8.34375 30.164062 0 8.339844 8.339843 8.339844 21.820312 0 30.164062zm0 0" fill="#fafafa"/></svg>
        <p class="form__popupSuccess-text">Клиент успешно создан!</p>
      </div>
    </transition>
  </form>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength } from 'vuelidate/lib/validators/'

export default {
  mixins: [validationMixin],
  name: 'form',
  data() {
    return {
      form: {
        lastName: '',
        firstName: '',
        middleName: '',
        birthDate: '',
        gender: 'male',
        tel: '',
        country: '',
        mailIndex: '',
        city: '',
        street: '',
        house: '',
        selectedGroup: [],
        selectedDoctor: 'Иванов',
        selectedTypeDocs: 'Паспорт',
        seriesDoc: '',
        numberDocs: '',
        dateGivenDocs: '',
        givenDocs: '',
        sms: false,
      },
      groupsPeople: [
        {
          label: 'VIP',
          value: 'vip'
        },
        {
          label: 'Проблемные',
          value: 'problem'
        },
        {
          label: 'ОМС',
          value: 'medical insurance'
        }
      ],
      doctors: ['Иванов', 'Захаров', 'Чернышева'],
      typeDocs: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
      submitSuccsess: false
    }
  },
  methods: {
    isActiveGroups() {
      return this.groupsPeople.length < 4 ? true : false
    },
    checkValid(event) {
      this.$v.form.$touch()
      if (!this.$v.form.$error) {
        this.submitSuccsess = true
        this.form.lastName = ''
        this.form.firstName = ''
        this.form.middleName = ''
        this.form.birthDate = ''
        this.form.gender = 'male'
        this.form.tel = ''
        this.form.country = ''
        this.form.mailIndex = ''
        this.form.city = ''
        this.form.street = ''
        this.form.house = ''
        this.form.selectedGroup = []
        this.form.selectedDoctor = 'Иванов'
        this.form.selectedTypeDocs = 'Паспорт'
        this.form.seriesDoc = ''
        this.form.numberDocs = ''
        this.form.dateGivenDocs = ''
        this.form.givenDocs = ''
        this.form.sms = false
        event.target.reset()
      } else {
        this.submitSuccsess = false
      }
    }

  },
  validations: {
    form: {
      firstName: {
        required,
      },
      lastName: {
        required,
      },
      birthDate: {
        required
      },
      tel: {
        required,
        minLength: minLength(11),
        vadidateTel(value) {
          return /^[7][0-9]{10}$/.test(value) ? true : false
        }
      },
      selectedGroup: {
        required
      },
      city: {
        required
      },
      selectedTypeDocs: {
        required
      },
      dateGivenDocs: {
        required
      }
    }
  }
}
</script>
