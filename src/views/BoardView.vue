<template>
  <div>
    <HeaderComponent />
    <BoardList v-if="listView" v-bind:list="boardList"
      @show-write="showWrite" @board-delete="deleteBoard"
      @show-read="showRead" />
    <BoardWrite v-if="writeView" v-on:save-board="saveBoard" />
    <BoardRead v-if="readView" v-bind:obj="board"
      @show-list="showList" />
    <!--html 전달-->
    <FooterComponent v-bind:data="htmlData">
      <template v-slot:footer><p>Since 2020</p></template>
      <template v-slot:header><h3>Header들어갈 내용</h3></template>
      <template v-slot:default><p>Hello, World</p></template>
    </FooterComponent>
  </div>
</template>

<script>
  import BoardList from '../components/BoardList.vue'
  import HeaderComponent from '../components/HeaderComponent.vue'
  import FooterComponent from '../components/FooterComponent.vue'
  import BoardWrite from '../components/BoardWrite.vue'
  import BoardRead from '../components/BoardRead.vue'

  export default {
    data() {
      return {
        listView: true,
        writeView: false,
        readView: false,
        board: {}, // 상세화면에 사용할 데이터.
        boardList: [{
            no: 1,
            title: 'Vue는 좋아요',
            content: '프레임워크입니다',
            view: 1
          },
          {
            no: 2,
            title: '좋은 아침입니다.',
            content: '금요일이네요',
            view: 3
          },
          {
            no: 3,
            title: '점심메뉴?',
            content: '국밥입니다.',
            view: 2
          }
        ],
        htmlData: '<p>Hello</p>'
      }
    },
    components: {
      BoardList,
      HeaderComponent,
      FooterComponent,
      BoardWrite,
      BoardRead
    },
    methods: {
      showWrite() { // 등록화면을 보여주기.
        this.listView = false;
        this.writeView = true;
        this.readView = false;
      },
      showList() { // 목록보여주는 기능.
        this.listView = true;
        this.writeView = false;
        this.readView = false;
      },
      showRead(board) { // 상세화면.
        console.log(board); // 글번호활용.
        this.listView = false;
        this.writeView = false;
        this.readView = true;

        this.board = board; // 매개값을 board 데이터에 대입.

        // 글번호의 조회(view) 증가.
        // 글번호의 인덱스를 찾아서 현재 조회수를 파악하고 증가하기.
        let idx = -1; // 해당위치의 값을 새로운 값으로 변경하기 위해서.
        let viewCnt = 0;
        for (let i = 0; i < this.boardList.length; i++) {
          if (this.boardList[i].no == board.no) {
            idx = i;
            viewCnt = this.boardList[i].view;
            break;
          }
        }
        let brd = {no: board.no, title: board.title, content: board.content, view: ++viewCnt};
        this.boardList.splice(idx, 1, brd); // 새로운 값으로 변경.
      },
      saveBoard(title, content) { // 신규글 등록.

        let idx = this.boardList.length - 1; // 마지막데이터의 index.
        let no = parseInt(this.boardList[idx].no) + 1; // 새로운데이터의 no.
        let board = {
          no,
          title,
          content,
          view: 0
        }
        this.boardList.splice(this.boardList.length, 0, board);
        this.showList();
      },
      deleteBoard(no) { // 글삭제.
        this.boardList = //
          this.boardList.filter(board => board.no == no ? false : true);
      }
    }
  }
</script>