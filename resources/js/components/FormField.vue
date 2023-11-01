<template>
    <DefaultField
        :field="field"
        :errors="errors"
        :show-help-text="showHelpText"
        :full-width-content="fullWidthContent"
    >
        <template #field>
            <select class="w-full relative flex items-center form-control form-input-bordered form-select pr-6" v-model="field.workingdays">
                <option value="select" selected>-</option>
                <option value="weekdays">Будние дни</option>
                <option value="holidays">Выходные</option>
                <option value="everyday">Каждый день</option>
                <option value="custom">Пользовательский</option>
            </select>

            <div v-if="field.workingdays != 'custom'">
                <div class="flex">
                    <p class="w-1/2 text-center" style="margin:auto" v-if="field.workingdays == 'weekdays'">Пн-Пт</p>
                    <p class="w-1/2 text-center" style="margin:auto" v-if="field.workingdays == 'holidays'">Сб-Вс</p>
                    <p class="w-1/2 text-center" style="margin:auto" v-if="field.workingdays == 'everyday'">Пн-Вс</p>
                    <input
                        v-if="field.workingdays != 'select'"
                        v-model="field.workingdaysdaysstart"
                        type="time"
                        class="w-full form-control form-input form-input-bordered"
                        :class="errorClasses"
                    />
                    <input
                        v-if="field.workingdays != 'select'"
                        v-model="field.workingdaysdaysend"
                        type="time"
                        class="w-full form-control form-input form-input-bordered"
                        :class="errorClasses"
                    />
                </div>
            </div>

            <div v-else>
                <template v-for="(label, day) in daysOfWeek" :key="day">
                    <div class="flex">
                        <p class="w-1/2 text-center" style="margin:auto">{{ label }}</p>
                        <input
                            v-model="field['workingdays' + day + 'start']"
                            type="time"
                            class="w-full form-control form-input form-input-bordered"
                            :class="errorClasses"
                        />
                        <input
                            v-model="field['workingdays' + day + 'end']"
                            type="time"
                            class="w-full form-control form-input form-input-bordered"
                            :class="errorClasses"
                        />
                    </div>
                </template>
            </div>
        </template>
    </DefaultField>
</template>


<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],
    data() {
        return {
            daysOfWeek: {
                monday: 'Пн',
                tuesday: 'Вт',
                wednesday: 'Ср',
                thursday: 'Чт',
                friday: 'Пт',
                saturday: 'Сб',
                sunday: 'Вс',
            }
        };
    },
    methods: {
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
            this.field.workingdays = 'select'
            this.field.workingdaysdaysstart = "09:00"
            this.field.workingdaysdaysend = "18:00"
            this.field.workingdaysmondaystart = "09:00"
            this.field.workingdaysmondayend = "18:00"
            this.field.workingdaystuesdaystart = "09:00"
            this.field.workingdaystuesdayend = "18:00"
            this.field.workingdayswednesdaystart = "09:00"
            this.field.workingdayswednesdayend = "18:00"
            this.field.workingdaysthursdaystart = "09:00"
            this.field.workingdaysthursdayend = "18:00"
            this.field.workingdaysfridaystart = "09:00"
            this.field.workingdaysfridayend = "18:00"
            this.field.workingdayssaturdaystart = "09:00"
            this.field.workingdayssaturdayend = "18:00"
            this.field.workingdayssundaystart = "09:00"
            this.field.workingdayssundayend = "18:00"
            this.value = this.field.value || ''
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            // Прямо добавляем объект 'field' в formData
            formData.append(this.fieldAttribute, JSON.stringify(this.field))
        },
    },
}
</script>
