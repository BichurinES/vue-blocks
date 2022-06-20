<template>
  <div id="app">
    <VueBlocksContainer
            @contextmenu.native="showContextMenu"
            @click.native="closeContextMenu"
            ref="container"
            :blocksContent="blocks"
            :scene.sync="scene"
            @blockSelect="selectBlock"
            @blockDeselect="deselectBlock"
            class="container"/>
<!--    <label>-->
<!--      <select name="type" v-model="selectedType">-->
<!--        <template v-for="type in selectBlocksType">-->
<!--          <optgroup :label="type">-->
<!--            <option v-for="block in filteredBlocks(type)" :value="block.name">{{block.title || block.name}}</option>-->
<!--          </optgroup>-->
<!--        </template>-->
<!--      </select>-->
<!--    </label>-->
<!--    <button @click.stop="addBlock">Add</button>-->
<!--    |-->
<!--    <label for="useContextMenu">-->
<!--      <input type="checkbox" v-model="useContextMenu" id="useContextMenu">Use right click for Add blocks-->
<!--    </label>-->

<!--    <ul id="contextMenu" ref="contextMenu" tabindex="-1" v-show="contextMenu.isShow"-->
<!--        @blur="closeContextMenu"-->
<!--        :style="{top: contextMenu.top + 'px', left: contextMenu.left + 'px'}">-->
<!--      <template v-for="type in selectBlocksType">-->
<!--        <li class="label">{{type}}</li>-->
<!--        <li v-for="block in filteredBlocks(type)"-->
<!--            @click="addBlockContextMenu(block.name)">{{block.title || block.name}}-->
<!--        </li>-->
<!--      </template>-->
<!--    </ul>-->
  </div>
</template>

<script>
  import merge from 'deepmerge'

  import VueBlocksContainer from './components/VueBlocksContainer'
  import VueBlockProperty from './components/VueBlockProperty'
  import domHelper from './helpers/dom'

  export default {
    name: 'App',
    components: {
      VueBlocksContainer,
      VueBlockProperty
    },
    data: function () {
      return {
        blocks: [
          {
            type: 'radio',
            title: 'Text',
            family: 'Animations',
            description: 'Show text',
            fields: [
              {
                name: 'text',
                label: 'Text',
                type: 'string',
                attr: 'property'
              },
              {
                name: 'delay',
                label: 'Delay (s)',
                type: 'number',
                attr: 'property'
              },
              {
                name: '',
                type: 'radio',
                attr: 'input'
              },
              {
                name: 'onShow',
                type: 'event',
                attr: 'output'
              },
              {
                name: 'onHide',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'checkbox',
            title: 'Animation',
            family: 'Animations',
            description: 'Show animation',
            fields: [
              {
                name: 'animation',
                label: 'Animation',
                type: 'animation',
                attr: 'property'
              },
              {
                name: '',
                type: 'checkbox',
                attr: 'input'
              },
              {
                name: 'onEnd',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'range',
            title: 'Animation',
            family: 'Animations',
            description: 'Show animation',
            fields: [
              {
                name: 'animation',
                label: 'Animation',
                type: 'animation',
                attr: 'property'
              },
              {
                name: '',
                type: 'range',
                attr: 'input'
              },
              {
                name: 'onEnd',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'textarea',
            title: 'Animation',
            family: 'Animations',
            description: 'Show animation',
            fields: [
              {
                name: 'animation',
                label: 'Animation',
                type: 'animation',
                attr: 'property'
              },
              {
                name: '',
                type: 'textarea',
                attr: 'input'
              },
              {
                name: 'onEnd',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'image',
            title: 'Animation',
            family: 'Animations',
            description: 'Show animation',
            fields: [
              {
                name: 'animation',
                label: 'Animation',
                type: 'animation',
                attr: 'property'
              },
              {
                name: '',
                type: 'file',
                attr: 'input'
              },
              {
                name: 'onEnd',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'Chat message',
            family: 'Events',
            description: '',
            fields: [
              {
                name: 'message',
                label: 'Activation message',
                type: 'string',
                attr: 'property'
              },
              {
                name: 'onMessage',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'delay',
            title: 'Delay',
            family: 'Time',
            description: '',
            fields: [
              {
                name: 'delay',
                label: 'Delay (s)',
                type: 'number',
                attr: 'property',
                value: 1.0
              },
              {
                name: 'input',
                type: 'event',
                attr: 'input'
              },
              {
                name: 'output',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'shortcuts',
            title: 'Shortcuts',
            family: 'Events',
            description: 'Press shortcut for call event',
            fields: [
              {
                name: 'keys',
                label: 'Activation keys',
                type: 'keys',
                attr: 'property'
              },
              {
                name: 'onPress',
                type: 'event',
                attr: 'output'
              }
            ]
          },
          {
            type: 'splitter',
            title: 'Splitter',
            family: 'Helpers',
            description: 'Press shortcut for call event',
            fields: [
              {
                name: 'input',
                type: 'event',
                attr: 'input'
              },
              {
                name: 'output',
                type: 'event',
                attr: 'output'
              },
              {
                name: 'output',
                type: 'event',
                attr: 'output'
              },
              {
                name: 'output',
                type: 'event',
                attr: 'output'
              },
              {
                name: 'output',
                type: 'event',
                attr: 'output'
              }
            ]
          }
        ],
        scene: {
          blocks: [
            {
              "id": 336,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Уточним ваш рост?",
              "stage": "Рост",
              "img": "",
              "type": "range",
              "selection_id": 6,
              "required": 1,
              "branch_id": 338,
              x: -800,
              y: 170,
              "answers": {
                "id": 546,
                "question_id": 336,
                "branch_id": null,
                "host_id": null,
                "answer": null,
                "selection": 0,
                "created_at": null,
                "updated_at": null,
                "image_answer": null,
                "min": null,
                "max": null,
                "initial_value": null,
                "division": null,
                "step": null,
                "mode": "value"
              }
            },
            {
              "id": 337,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Укажите рост ребенка",
              "stage": "Рост ребенка",
              "img": "",
              "type": "range",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -800,
              y: 450,
              "answers": {
                "id": 575,
                "question_id": 337,
                "branch_id": null,
                "host_id": null,
                "answer": null,
                "selection": 0,
                "created_at": null,
                "updated_at": null,
                "image_answer": null,
                "min": null,
                "max": null,
                "initial_value": null,
                "division": null,
                "step": null,
                "mode": "value"
              }
            },
            {
              "id": 338,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Определим, где будем кататься",
              "stage": "Тип покрытия",
              "img": "",
              "type": "checkbox",
              "selection_id": 6,
              "required": 1,
              "branch_id": 339,
              x: -690,
              y: -110,
              "answers": [
                {
                  "id": 551,
                  "question_id": 338,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "По городу",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 552,
                  "question_id": 338,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "По бездорожью",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 553,
                  "question_id": 338,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "По ровному асфальту на большие расстояния",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 554,
                  "question_id": 338,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Меня интересует велосипед для трюков",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 339,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Укажите пол",
              "stage": "Пол",
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -390,
              y: -110,
              "answers": [
                {
                  "id": 555,
                  "question_id": 339,
                  "branch_id": 340,
                  "host_id": null,
                  "answer": "Мужской",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 556,
                  "question_id": 339,
                  "branch_id": 341,
                  "host_id": null,
                  "answer": "Женский",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 340,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Выберет свой вес",
              "stage": "Вес",
              "img": "",
              "type": "range",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -90,
              y: -110,
              "answers": {
                "id": 557,
                "question_id": 340,
                "branch_id": null,
                "host_id": null,
                "answer": null,
                "selection": 0,
                "created_at": null,
                "updated_at": null,
                "image_answer": null,
                "min": null,
                "max": null,
                "initial_value": null,
                "division": null,
                "step": null,
                "mode": "value"
              }
            },
            {
              "id": 341,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Выберете желаемый цвет",
              "stage": "Цвет",
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -450,
              y: 70,
              "answers": [
                {
                  "id": 564,
                  "question_id": 341,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Красный",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 565,
                  "question_id": 341,
                  "branch_id": 342,
                  "host_id": null,
                  "answer": "Белый",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 566,
                  "question_id": 341,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Синий",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 342,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Вы уверены?",
              "stage": "Уверены?",
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -200,
              y: 70,
              "answers": [
                {
                  "id": 567,
                  "question_id": 342,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Да",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 568,
                  "question_id": 342,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Нет",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 344,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Сколько скоростей интересует",
              "stage": null,
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: 300,
              y: 208,
              "answers": [
                {
                  "id": 571,
                  "question_id": 344,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "До 20",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 572,
                  "question_id": 344,
                  "branch_id": 345,
                  "host_id": null,
                  "answer": "Больше 20",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 345,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Какую сумму вы готовы выделить на покупку?",
              "stage": "Стоимость",
              "img": "",
              "type": "range",
              "selection_id": 6,
              "required": 1,
              "branch_id": 347,
              x: 550,
              y: 208,
              "answers": {
                "id": 573,
                "question_id": 345,
                "branch_id": null,
                "host_id": null,
                "answer": null,
                "selection": 0,
                "created_at": null,
                "updated_at": null,
                "image_answer": null,
                "min": null,
                "max": null,
                "initial_value": null,
                "division": null,
                "step": null,
                "mode": "value"
              }
            },
            {
              "id": 347,
              "host_id": null,
              "branch": 1,
              "index": null,
              "title": "Оставьте дополнительные пожелания",
              "stage": "Пожелания",
              "img": "",
              "type": "textarea",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              "answers": null,
              x: -50,
              y: 430
            },
            {
              "id": 335,
              "host_id": null,
              "branch": 0,
              "index": 1,
              "title": "Кому подбираем велосипед?",
              "stage": "Возраст",
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: -1030,
              y: 300,
              "answers": [
                {
                  "id": 544,
                  "question_id": 335,
                  "branch_id": 336,
                  "host_id": null,
                  "answer": "Для взрослого",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 545,
                  "question_id": 335,
                  "branch_id": 337,
                  "host_id": null,
                  "answer": "Для ребенка",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 343,
              "host_id": null,
              "branch": 0,
              "index": 2,
              "title": "Сколько скоростей интересует",
              "stage": null,
              "img": "",
              "type": "radio",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              x: 53,
              y: 208,
              "answers": [
                {
                  "id": 569,
                  "question_id": 343,
                  "branch_id": null,
                  "host_id": null,
                  "answer": "Одна",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                },
                {
                  "id": 570,
                  "question_id": 343,
                  "branch_id": 344,
                  "host_id": null,
                  "answer": "Много",
                  "selection": 1,
                  "img": "",
                  "min": null,
                  "max": null,
                  "step": null,
                  "initial_value": null
                }
              ]
            },
            {
              "id": 346,
              "host_id": null,
              "branch": 0,
              "index": 3,
              "title": "Разместите фото",
              "stage": "Фото",
              "img": "",
              "type": "image",
              "selection_id": 6,
              "required": 1,
              "branch_id": null,
              "answers": null,
              x: 250,
              y: 430
            }
          ],
            // {
            //   id: 2,
            //   x: -1000,
            //   y: -69,
            //   type: 'radio',
            //   title: 'Обратим внимание на количество скоростей',
            //   answers: [
            //     { answer: "Фиксед / синглспид" }, { answer: "Скоростной велосипед" }
            //   ]
            // },
            // {
            //   id: 4,
            //   x: -557,
            //   y: -68.5,
            //   type: 'checkbox',
            //   title: 'Определим, где будем кататься',
            //   answers: [
            //     { answer: "По городу" }, { answer: "По бездорожью" }, { answer: "По трассе" }, { answer: "Для трюков" }
            //   ]
            // }
          links: [],
          container: {
            centerX: 933,
            centerY: 172,
            scale: .9
          }
        },
        selectedBlock: null,
        selectedType: 'delay',
        useContextMenu: true,
        contextMenu: {
          isShow: false,
          mouseX: 0,
          mouseY: 0,
          top: 0,
          left: 0
        },
        questions: [
          {
            "id": 336,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Уточним ваш рост?",
            "stage": "Рост",
            "img": "",
            "type": "range",
            "selection_id": 6,
            "required": 1,
            "branch_id": 338,
            "answers": {
              "id": 546,
              "question_id": 336,
              "branch_id": null,
              "host_id": null,
              "answer": null,
              "selection": 0,
              "created_at": null,
              "updated_at": null,
              "image_answer": null,
              "min": null,
              "max": null,
              "initial_value": null,
              "division": null,
              "step": null,
              "mode": "value"
            }
          },
          {
            "id": 337,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Укажите рост ребенка",
            "stage": "Рост ребенка",
            "img": "",
            "type": "range",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": {
              "id": 575,
              "question_id": 337,
              "branch_id": null,
              "host_id": null,
              "answer": null,
              "selection": 0,
              "created_at": null,
              "updated_at": null,
              "image_answer": null,
              "min": null,
              "max": null,
              "initial_value": null,
              "division": null,
              "step": null,
              "mode": "value"
            }
          },
          {
            "id": 338,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Определим, где будем кататься",
            "stage": "Тип покрытия",
            "img": "",
            "type": "checkbox",
            "selection_id": 6,
            "required": 1,
            "branch_id": 339,
            "answers": [
              {
                "id": 551,
                "question_id": 338,
                "branch_id": null,
                "host_id": null,
                "answer": "По городу",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 552,
                "question_id": 338,
                "branch_id": null,
                "host_id": null,
                "answer": "По бездорожью",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 553,
                "question_id": 338,
                "branch_id": null,
                "host_id": null,
                "answer": "По ровному асфальту на большие расстояния",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 554,
                "question_id": 338,
                "branch_id": null,
                "host_id": null,
                "answer": "Меня интересует велосипед для трюков",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 339,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Укажите пол",
            "stage": "Пол",
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 555,
                "question_id": 339,
                "branch_id": 340,
                "host_id": null,
                "answer": "Мужской",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 556,
                "question_id": 339,
                "branch_id": 341,
                "host_id": null,
                "answer": "Женский",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 340,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Выберет свой вес",
            "stage": "Вес",
            "img": "",
            "type": "range",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": {
              "id": 557,
              "question_id": 340,
              "branch_id": null,
              "host_id": null,
              "answer": null,
              "selection": 0,
              "created_at": null,
              "updated_at": null,
              "image_answer": null,
              "min": null,
              "max": null,
              "initial_value": null,
              "division": null,
              "step": null,
              "mode": "value"
            }
          },
          {
            "id": 341,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Выберете желаемый цвет",
            "stage": "Цвет",
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 564,
                "question_id": 341,
                "branch_id": null,
                "host_id": null,
                "answer": "Красный",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 565,
                "question_id": 341,
                "branch_id": 342,
                "host_id": null,
                "answer": "Белый",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 566,
                "question_id": 341,
                "branch_id": null,
                "host_id": null,
                "answer": "Синий",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 342,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Вы уверены?",
            "stage": "Уверены?",
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 567,
                "question_id": 342,
                "branch_id": null,
                "host_id": null,
                "answer": "Да",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 568,
                "question_id": 342,
                "branch_id": null,
                "host_id": null,
                "answer": "Нет",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 344,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Сколько скоростей интересует",
            "stage": null,
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 571,
                "question_id": 344,
                "branch_id": null,
                "host_id": null,
                "answer": "До 20",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 572,
                "question_id": 344,
                "branch_id": 345,
                "host_id": null,
                "answer": "Больше 20",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 345,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Какую сумму вы готовы выделить на покупку?",
            "stage": "Стоимость",
            "img": "",
            "type": "range",
            "selection_id": 6,
            "required": 1,
            "branch_id": 347,
            "answers": {
              "id": 573,
              "question_id": 345,
              "branch_id": null,
              "host_id": null,
              "answer": null,
              "selection": 0,
              "created_at": null,
              "updated_at": null,
              "image_answer": null,
              "min": null,
              "max": null,
              "initial_value": null,
              "division": null,
              "step": null,
              "mode": "value"
            }
          },
          {
            "id": 347,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Оставьте дополнительные пожелания",
            "stage": "Пожелания",
            "img": "",
            "type": "textarea",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": null
          },
          {
            "id": 335,
            "host_id": null,
            "branch": 0,
            "index": 1,
            "title": "Кому подбираем велосипед?",
            "stage": "Возраст",
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 544,
                "question_id": 335,
                "branch_id": 336,
                "host_id": null,
                "answer": "Для взрослого",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 545,
                "question_id": 335,
                "branch_id": 337,
                "host_id": null,
                "answer": "Для ребенка",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 343,
            "host_id": null,
            "branch": 0,
            "index": 2,
            "title": "Сколько скоростей интересует",
            "stage": null,
            "img": "",
            "type": "radio",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": [
              {
                "id": 569,
                "question_id": 343,
                "branch_id": null,
                "host_id": null,
                "answer": "Одна",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              },
              {
                "id": 570,
                "question_id": 343,
                "branch_id": 344,
                "host_id": null,
                "answer": "Много",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null
              }
            ]
          },
          {
            "id": 346,
            "host_id": null,
            "branch": 0,
            "index": 3,
            "title": "Разместите фото",
            "stage": "Фото",
            "img": "",
            "type": "image",
            "selection_id": 6,
            "required": 1,
            "branch_id": null,
            "answers": null
          }
        ]
      }
    },
    computed: {
      selectedBlockProperty () {
        if (!this.selectedBlock || !this.selectedBlock.values || !this.selectedBlock.values.property) {
          return null
        }

        return this.selectedBlock.values.property
      },
      selectBlocksType () {
        return this.blocks.map(b => {
          return b.family
        }).filter((value, index, array) => {
          return array.indexOf(value) === index
        })
      }
    },
    methods: {
      selectBlock (block) {
        console.log('select', block)
        this.selectedBlock = block
      },
      deselectBlock (block) {
        console.log('deselect', block)
        this.selectedBlock = null
      },
      filteredBlocks (type) {
        return this.blocks.filter(value => {
          return value.family === type
        })
      },
      addBlock () {
        console.log(this.selectedType)
        this.$refs.container.addNewBlock(this.selectedType)
      },
      saveProperty (val) {
        console.log(val)

        let scene = this.scene
        let block = scene.blocks.find(b => {
          return b.id === this.selectedBlock.id
        })
        block.values.property = val

        this.scene = merge({}, scene)
      },
      showContextMenu (e) {
        if (!this.useContextMenu) return
        if (e.preventDefault) e.preventDefault()

        this.contextMenu.isShow = true
        this.contextMenu.mouseX = e.x
        this.contextMenu.mouseY = e.y

        this.$nextTick(function () {
          this.setMenu(e.y, e.x)
          this.$refs.contextMenu.focus()
        })
      },
      setMenu (top, left) {
        let border = 5
        let contextMenuEl = this.$refs.contextMenu
        let containerElRect = this.$refs.container.$el.getBoundingClientRect()
        let largestWidth = containerElRect.right - contextMenuEl.offsetWidth - border
        let largestHeight = containerElRect.bottom - contextMenuEl.offsetHeight - border

        console.log(this.$refs.container)
        console.log(containerElRect)

        if (left > largestWidth) left = largestWidth
        if (top > largestHeight) top = largestHeight

        this.contextMenu.top = top
        this.contextMenu.left = left
      },
      addBlockContextMenu (name) {
        let offset = domHelper.getOffsetRect(this.$refs.container.$el)
        let x = this.contextMenu.mouseX - offset.left
        let y = this.contextMenu.mouseY - offset.top

        this.$refs.container.addNewBlock(name, x, y)
        this.closeContextMenu()
      },
      closeContextMenu () {
        this.contextMenu.isShow = false
      },
      createLinks (questions) {
        // ВСПОМОГАТЕЛЬНЫЕ ОБЪЕКТЫ И МАССИВЫ
        // Массивы вопросов: основная линия и ветки
        const branchQuestions = []
        const mainQuestions = []

        // Объект id вопросов, который идут главной линией (не внутри веток)
        // ключами являются их порядковые index
        let mainQuestionsId = {}
        // Отсортированный массив с индексами вопросов,
        // чтобы проще находить следующий основной вопрос
        let questionIndexes = []

        // Объект с вопросами, заканчивающих ветки, и хранящие индекс вопроса из основной линии,
        // который создал ветку
        const branchParentIndexes = {}

        // Функция поиска ближайщего потомка, создавшего ветку,
        // и сохранение его индекса в соответствующий объект
        function findParentIndex (id, originId) {
          // Сначала ищем среди основной линии, так как есть вероятность ранннего выхода
          const mainParent = mainQuestions.find((mainQuestion) => {
            return mainQuestion.branch_id === id
                || (Array.isArray(mainQuestion.answers) && mainQuestion.answers?.some(mainQuestionAnswer => mainQuestionAnswer.branch_id === id))
                || mainQuestion.answers?.branch_id === id
          })
          console.log(id, mainParent)
          if (mainParent) {
            branchParentIndexes[originId || id] = mainParent.index
            return
          }
          // Если не нашли в основной линии, ищем среди других веток, и далее повторяем,
          // пока не найдем родителя из основной линии
          const branchParent = branchQuestions.find((branchQuestion) => {
            return branchQuestion.branch_id === id
                || (Array.isArray(branchQuestion.answers) && branchQuestion.answers?.some(branchQuestionAnswer => branchQuestionAnswer.branch_id === id))
                || branchQuestion.answers?.branch_id === id
          })
          if (branchParent) {
            return findParentIndex(branchParent.id, originId || id)
          }
        }

        // Наполнение вспомогательных объектов и массивов
        // (mainQuestions, mainQuestionsId, questionIndexes, branchQuestions)
        questions.forEach((item) => {
          if (item.index) {
            mainQuestions.push(item)
            mainQuestionsId[item.index] = item.id
            questionIndexes.push(item.index)
            return
          }
          if (+item.branch) {
            branchQuestions.push(item)
            return
          }
        })

        // сортировка индексов по порядку
        questionIndexes.sort()

        // Сохранение информации о родителях из основной ветки для вопросов,
        // заканчивающих ветки
        branchQuestions.forEach(({ id, answers, branch_id }) => {
          if (!branch_id && Array.isArray(answers) && !answers?.some(({ branch_id }) => branch_id)) {
            findParentIndex(id)
          }
          if (!answers?.branch_id) {
            findParentIndex(id)
          }
        })

        // СОБИРАЕМ ОБЪЕКТ ЛИНИЙ
        return questions.reduce((res, { id, branch_id, answers, index }) => {
          const targetSlot = 0
          const originID = id
          let targetID = branch_id

          // Функция для добавления новой связи в результат
          function addLink (_, i) {
            res.push({
              id: res.length + 1,
              originID,
              originSlot: i,
              targetID,
              targetSlot
            })
          }

          // вопросы, у которых нет поля ответов или там не массив
          if (!answers || !Array.isArray(answers)) {
            console.log(id, branchParentIndexes, originID)
            if (!targetID) {
              if (index) {
                const nextIndex = questionIndexes[questionIndexes.indexOf(index) + 1]
                targetID = mainQuestionsId[nextIndex]
              }
              if (branchParentIndexes[originID]) {
                const nextIndex = questionIndexes[questionIndexes.indexOf(branchParentIndexes[originID]) + 1]
                targetID = mainQuestionsId[nextIndex]
              }
            }
            addLink(null, 0)
            return res;
          }

          // вопросы, у которых нет информации о следующей цели (либо окончание веток,
          // либо вопрос из основной линии, либо конец всей подборки)

          if (!branch_id && !answers?.some(({ branch_id }) => branch_id > 0)) {
            if (index) {
              const nextIndex = questionIndexes[questionIndexes.indexOf(index) + 1]
              targetID = mainQuestionsId[nextIndex]
            }
            if (branchParentIndexes[originID]) {
              const nextIndex = questionIndexes[questionIndexes.indexOf(branchParentIndexes[originID]) + 1]
              targetID = mainQuestionsId[nextIndex]
            }
            answers.forEach(addLink)
            return res
          }

          // вопросы с веткой (не от каждого отдельного ответа)
          if (targetID) {
            answers.forEach(addLink)
            return res
          }

          // вопросы с веткой от каждого отдельного ответа
          answers.forEach(({ branch_id }, i) => {
            targetID = branch_id
            if (targetID) {
              addLink(null, i)
            }
          })
          return res
        }, [])
      }
    },
    beforeMount() {
      this.scene.links = this.createLinks(this.questions)
      console.log(this.scene.links)
    }
  }
</script>

<style lang="less">
  html, body {
    margin: 0;
    padding: 0;
  }

  html {
    width: 100vw;
    height: 100vh;
  }

  body {
    width: 100%;
    height: 100%;
  }

  #app {
    width: 100%;
    height: 100%;
  }

  .container {
    width: 100%;
    height: 100%;
  }

  #contextMenu {
    position: absolute;
    z-index: 1000;
    background: white;
    padding: 5px;
    margin: 0;

    li {
      &.label {
        color: gray;
        font-size: 90%;
      }
      list-style: none;
    }

    &:focus {
      outline: none;
    }
  }
</style>
