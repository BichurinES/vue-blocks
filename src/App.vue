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
    <label>
      <select name="type" v-model="selectedType">
        <template v-for="type in selectBlocksType">
          <optgroup :label="type">
            <option v-for="block in filteredBlocks(type)" :value="block.name">{{block.title || block.name}}</option>
          </optgroup>
        </template>
      </select>
    </label>
    <button @click.stop="addBlock">Add</button>
    |
    <label for="useContextMenu">
      <input type="checkbox" v-model="useContextMenu" id="useContextMenu">Use right click for Add blocks
    </label>

    <ul id="contextMenu" ref="contextMenu" tabindex="-1" v-show="contextMenu.isShow"
        @blur="closeContextMenu"
        :style="{top: contextMenu.top + 'px', left: contextMenu.left + 'px'}">
      <template v-for="type in selectBlocksType">
        <li class="label">{{type}}</li>
        <li v-for="block in filteredBlocks(type)"
            @click="addBlockContextMenu(block.name)">{{block.title || block.name}}
        </li>
      </template>
    </ul>
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
            answers: [
              { answer: "Фиксед / синглспид" }, { answer: "Скоростной велосипед" }
            ],
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
            type: 'file',
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
              "0": {
                "id": 220,
                "host_id": null,
                "branch": 1,
                "index": null,
                "title": "Определим, где будем кататься",
                "stage": "Тип покрытия",
                "img": "",
                "type": "checkbox",
                "selection_id": 4,
                "required": 1,
                "branch_id": 221,
                "answers": [
                  {
                    "id": 342,
                    "question_id": 220,
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
                    "id": 343,
                    "question_id": 220,
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
                    "id": 344,
                    "question_id": 220,
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
                    "id": 345,
                    "question_id": 220,
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
              "1": {
                "id": 221,
                "host_id": null,
                "branch": 1,
                "index": null,
                "title": "Уточним ваш рост?",
                "stage": "Рост",
                "img": "",
                "type": "range",
                "selection_id": 4,
                "required": 1,
                "branch_id": null,
                "answers": {
                  "id": 346,
                  "question_id": 221,
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
              "2": {
                "id": 222,
                "host_id": null,
                "branch": 1,
                "index": null,
                "title": "Уточним рост ребенка?",
                "stage": "Рост ребенка",
                "img": "",
                "type": "range",
                "selection_id": 4,
                "required": 1,
                "branch_id": null,
                "answers": {
                  "id": 347,
                  "question_id": 222,
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
              },{
                "id": 219,
                "host_id": null,
                "branch": 0,
                "index": 1,
                "title": "Кому подбираем велосипед?",
                "stage": "Возраст",
                "img": "",
                "type": "radio",
                "selection_id": 4,
                "required": 1,
                "branch_id": null,
                "answers": [
                  {
                    "id": 340,
                    "question_id": 219,
                    "branch_id": 220,
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
                    "id": 341,
                    "question_id": 219,
                    "branch_id": 222,
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
              }
              ]
            },
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
          ],
          links: [],
          container: {
            centerX: 1042,
            centerY: 140,
            scale: 1
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
            id: 162,
            host_id: null,
            branch: 1,
            index: null,
            title: "Определим, где будем кататься",
            stage: "Тип покрытия",
            img: "",
            type: "checkbox",
            selection_id: 1,
            required: 1,
            branch_id: 163,
            x: -1000,
            y: -69,
            answers: [
              {
                "id": 250,
                "question_id": 162,
                "branch_id": null,
                "host_id": null,
                "answer": "По городу",
                "selection": 1,
                "img": "",
                "min": null,
                "max": null,
                "step": null,
                "initial_value": null,
                x: -800,
                y: -200,
              },
              {
                "id": 251,
                "question_id": 162,
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
                "id": 252,
                "question_id": 162,
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
                "id": 253,
                "question_id": 162,
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
            "id": 162,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Уточним ваш рост?",
            "stage": "Рост",
            "img": "",
            "type": "range",
            "selection_id ": 1,
            "required": 1,
            "branch_id": 250,
            "answers": null,
            x: -900,
            y: -200,
          },
          {
            "id": 164,
            "host_id": null,
            "branch": 1,
            "index": null,
            "title": "Уточним рост ребенка?",
            "stage": "Рост ребенка",
            "img": "",
            "type": "range",
            "selection_id ": 1,
            "required": 1,
            "branch_id": null,
            x: -900,
            y: -600,
            "answers": [{
              "id": 249,
              "question_id": 1,
              "branch_id": 164,
              "host_id": null,
              "answer": "Для ребенка",
              "selection": 1,
              "created_at": null,
              "updated_at": null,
              "image_answer": null,
              "min": null,
              "max": null,
              "initial_value": null,
              "division": null,
              "step": null,
              "mode": "value"
            }]
          },
          {
            "id": 1,
            "host_id": null,
            "branch": 0,
            "index": 1,
            "title": "Кому подбираем велосипед?",
            "stage": "Возраст",
            "img": "",
            "type": "radio",
            "selection_id ": 1,
            "required": 1,
            "branch_id": null,
            x: -1000,
            y: -400,
            "answers": [
              {
                "id": 248,
                "question_id": 1,
                "branch_id": 162,
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
                "id": 249,
                "question_id": 1,
                "branch_id": 164,
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
            "id": 165,
            "host_id": null,
            "branch": 0,
            "index": 4,
            "title": "Оставьте свои пожелания",
            "stage": "Пожелания",
            "img": "",
            "type": "textarea",
            "selection_id ": 1,
            "required": 1,
            "branch_id": null,
            "answers": null,
            x: -600,
            y: -400
          },
          {
            "id": 166,
            "host_id": null,
            "branch": 0,
            "index": 5,
            "title": "Если вы мечтаете о какой-то конкретной модели, можете оставить фото велосипеда",
            "stage": "Фото мечты",
            "img": "",
            "type": "file",
            "selection_id ": 1,
            "required": 1,
            "branch_id": null,
            "answers": null,
            x: -500,
            y: -400
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
        // Объект id вопросов, который идут главной линией (не внутри веток)
        // ключами являются их порядковые index
        let unbranchedQuestionsId = null;
        // Индексы таких вопросов
        let questionIndexes = [];

        // наполнение объектов
        unbranchedQuestionsId = this.questions.reduce((res, { index, id }) => {
          if (index > 0) {
            questionIndexes = [...questionIndexes, index]
            res[index] = id
          }
          return res
        }, {})

        // сортировка индексов по порядку
        questionIndexes.sort()
        let nextId = 0

        // собираем объект линий
        return this.questions.reduce((res, { id, branch_id, answers }) => {
          const targetSlot = 0
          const originID = id
          let targetID = branch_id
          // вопросы, которые не содержат поля - answers
          if (!answers) {
            if (!targetID) {
              targetID = unbranchedQuestionsId[questionIndexes[nextId]]
              nextId++
            }
            res.push({
              id: res.length + 1,
              originID,
              originSlot: 0,
              targetID,
              targetSlot
            })
            return res;
          }

          // вопросы у которых нет отклонений на ветки
          if (!branch_id && !answers?.some(({ branch_id }) => branch_id > 0)) {
            answers.forEach((_, i) => {
              res.push({
                id: res.length + 1,
                originID,
                originSlot: i,
                targetID: unbranchedQuestionsId[questionIndexes[nextId]],
                targetSlot
              })
            })
            nextId++
            return res
          }

          // вопросы с веткой (не от каждого отдельного ответа)
          if (targetID) {

            answers.forEach((_, i) => {
              res.push({
                id: res.length + 1,
                originID,
                originSlot: i,
                targetID,
                targetSlot
              })
            })
            return res
          }

          // вопросы с веткой от каждого отдельного ответа
          answers.forEach(({ branch_id }, i) => {
            res.push({
              id: res.length + 1,
              originID,
              originSlot: i,
              targetID: branch_id,
              targetSlot
            })
          })
          return res
        }, [])
      }
    },
    watch: {
      blocks (newValue) {
        console.log('blocks', JSON.stringify(newValue))
      },
      scene (newValue) {
        console.log('scene', JSON.stringify(newValue))
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
    width: ~"calc(100% - 40px)";
    height: ~"calc(100% - 40px)";
    padding: 20px 0 0 20px;
  }

  .container {
    width: 100%;
    height: ~"calc(100% - 50px)";
    border: 1px solid black;
  }

  #contextMenu {
    position: absolute;
    z-index: 1000;
    background: white;
    border: 1px solid black;
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
