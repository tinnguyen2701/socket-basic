Khi cần trao đổi dữ liệu cho nhau thì 2 ứng dụng cần phải biết thông tin IP và port bao nhiêu của ứng dụng kia.

# Danh sách những lệnh Emit trong Socket IO
function onConnect(socket){
  // Gửi cho tất cả client
  socket.emit('hello', 'can you hear me?', 1, 2, 'abc');

  // Gửi cho tất cả client ngoại trừ người gửi
  socket.broadcast.emit('broadcast', 'hello friends!');

  // Gửi cho tất cả client trong room 'game' ngoại trừ người gửi
  socket.to('game').emit('nice game', "let's play a game");

  // Gửi cho tất cả client trong room 'game1' và room 'game2' ngoại trừ người gửi
  socket.to('game1').to('game2').emit('nice game', "let's play a game (too)");

  //  Gửi cho tất cả client trong room 'game' bao gồm cả người gửi
  io.in('game').emit('big-announcement', 'the game will start soon');

  // Gửi cho tất cả client trong namespace 'myNamespace', bao gồm cả người gửi
  io.of('myNamespace').emit('bigger-announcement', 'the tournament will start soon');

  // Gửi cho room 'room' trong namespace 'myNamespace', bao gồm cả người gửi
  io.of('myNamespace').to('room').emit('event', 'message');

  // Gửi tin nhắn riêng cho socket đó qua socketId
  io.to(`${socketId}`).emit('hey', 'I just met you');

  // Gửi không đính kèm file nén
  socket.compress(false).emit('uncompressed', "that's rough");

  // Việc gửi tin nhắn này cần sự chấp nhận từ client thì mới có thể đến được client
  socket.volatile.emit('maybe', 'do you really need it?');

  // Gửi dữ liệu liên quan đến hệ nhị phân
  socket.binary(false).emit('what', 'I have no binaries!');

  // Gửi dữ liệu cho tất cả client sử dụng node
  io.local.emit('hi', 'my lovely babies');

  // Gửi đến tất cả client kết nối đến
  io.emit('an event sent to all connected clients');

};

# function
1. Lấy tất cả socketId trong 1 name space
const io = require('socket.io')();
io.of('/chat').clients((error, clients) => {
  if (error) throw error;
  console.log(clients); // => [PZDoMHjiu8PYfRiKAAAF, Anw2LatarvGVVXEIAAAD]
});
2. Lấy tất cả socketId trong 1 room
io.in(roomId).clients((err,clis)) => {
        console.log(clis);
});
3. Lấy tất cả room mà socket đó join
let roomIds = socket.rooms;
4. Lấy instance socket với socketId
let socket = io.connected[id];
5. Lấy instance Room từ roomId
let roomObj = io.nsps[namespace].adapter.rooms[room];
6. Join multiple room
socket.join(rooms[, callback]) // rooms is Array, not use for
ex: socket.join(user.room);
7. Ack sending
socket.emit('question', 'do you think so?', (answer) => {}); // callback được gọi khi bên nhận đã lấy được data
8. gửi dữ liệu nén
socket.compress(true).emit('compressed', "this is compress");
9. Gửi message có thể bị hủy bỏ nếu client chưa sãn sàng nhận
socket.volatile.emit('maybe', 'do you really need it?');
10. Gửi đến client trong 1 node
io.local.emit('hi', 'Hello my clients in this node');

# Using in repo
1. Lắng nghe
socket.on('join room', (data) => {});
2. Chỉ có socket hiện tại mới nhận được 
socket.emit('welcome', data);
3. Chỉ có các socket khác ngoại trừ socket hiện tại mới nhận đc
socket.broadcast.to(user.room).emit('join', data);
4. Chỉ tất cả socket trong room mới nhận được
io.to(user.room).emit('roomUser', data);

