<template>
  <div class="main">
    <div class="chat-sidebar">
      <div class="chat-header">
        <h3 class="chat-header-title">{{ streamTitle }}</h3>
        <span class="chatters">{{ chattersCount }}</span>
      </div>
      <div class="chat-content">
        <Chat id="messageContainer" />
      </div>
      <div class="chat-footer">
        <div class="chat-box-outter">
          <ChatSend />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Chat from "../components/Chat/PopoutChat";
import ChatSend from "../components/Chat/ChatSend";

export default {
  name: "NDA Chat",
  data() {
    return {
      ...this.mapData(core => ({
        lastMessage: core.messages.lastMessage,
        chatters: core.socket.chatters,
        chattersCount: core.socket.chattersCount,
        streamUrl: core.stream.url,
        streamTitle: core.stream.title
      })),
      message: ""
    };
  },
  components: {
    Chat,
    ChatSend
  },
  mounted() {
    this.$socket.connect(this.$el.querySelector("#messageContainer"));
  },
  methods: {
    async sendMessage() {
      if (this.message == "") return;

      this.$messages.send(this.message);
      this.message = "";
      setTimeout(() => {
        let container = this.$el.querySelector("#messageContainer");
        container.scrollTop = container.scrollHeight;
      }, 2);

      return true;
    }
  }
};
</script>

<style lang="scss">
.chat-sidebar {
  margin: auto;
  width: 23%;
  height: 100%;
  position: absolute;
  top: 0;
  right: 0;
  z-index: 10000;
  background-color: #1b1b1b;
  border-color: #bebebe00;
  width: -webkit-fill-available;
}

.chat-content {
  width: -webkit-fill-available;
}

.chat-header {
  background-color: #0a0a0a;
  border-color: #bebebe00;
  border-bottom: 1px;
  z-index: 20000;
  border-bottom-style: outset;
  display: flex;
  margin: auto;
  width: 100%;
  height: 4%;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
}

.chat-header .chat-header-title {
  color: #ffffff;
  display: flex;
  margin: auto;
  font-size: 13px;
}

.chat-box {
  position: absolute;
  display: flex;
  height: 15%;
  width: -webkit-fill-available;
  margin-top: 14px;
  margin-left: 2%;
  margin-right: 2%;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  color: #ffffff;
  background-color: #1d1d1d;
  border-color: #5252521f;
  border-style: solid;
  border-radius: 2px;
  padding: 10px 10px 10px 10px;
}

.chat-footer {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 10%;
  display: flex;
  border-color: #bebebe00;
  border-top: 1px;
  border-top-style: inset;
  width: -webkit-fill-available;
}

.chat-send {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  height: 25%;
  width: -webkit-fill-available;
  background: #442dad;
  color: white;
  border: 0;
  margin-left: 2%;
  margin-right: 2%;
  margin-bottom: 1%;
  border-radius: 2px;
  text-decoration: none;
  cursor: pointer;
  &:focus {
    outline: none;
  }
  &:hover {
    background-color: #2d1e70;
  }
  &:active {
    background-color: #2d1e70;
    box-shadow: 0 1px #2d1e70;
    transform: translateY(-0.9px);
  }
}

.chatters {
  margin-left: -15px;
  margin-right: 15px;
  margin-top: 12px;
  font-size: 14px;
}
</style>