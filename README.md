# SLQ
三连棋
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="js/react.js"></script>
    <script src="js/react-dom.js"></script>
    <script src="js/browser.min.js"></script>
    <style>
        body {
            font: 14px "Century Gothic", Futura, sans-serif;
            margin: 20px;
        }
        ol, ul {
            padding-left: 30px;
        }
        .board-row:after {
            clear: both;
            content: "";
            display: table;
        }
        .status {
            margin-bottom: 10px;
        }
        .square {
            background: #fff;
            border: 1px solid #999;
            float: left;
            font-size: 24px;
            font-weight: bold;
            line-height: 34px;
            height: 34px;
            margin-right: -1px;
            margin-top: -1px;
            padding: 0;
            text-align: center;
            width: 34px;
        }
        .square:focus {
            outline: none;
        }
        .kbd-navigation .square:focus {
            background: #ddd;
        }
        .game {
            display: flex;
            flex-direction: row;
        }
        .game-info {
            margin-left: 20px;
        }
    </style>
</head>
<body>
<div id="container"></div>
</body>

<script type="text/babel">
    var Square = React.createClass({
        handelClick(){
            //获取通过属性myProps所传递来的方法
            this.props.myProps(this.props.index);
        },
        render() {
        return (
                <button className="square" onClick={this.handelClick}>
                    {this.props.list[this.props.index]}
                </button>
        );
    }});
    var Board = React.createClass({
        getInitialState: function () {
            return {
                squares:Array(9).fill(null),
                xIsNext: true
            }
        },
        componentDidUpdate(){
            var isWinner = calculateWinner(this.state.squares);
            console.log(isWinner);
            if(isWinner){
                alert(isWinner+"赢了");
            }else{
                //squares数组没有null
                if(this.state.squares.indexOf(null) == -1){
                    alert("平局");
                }
            }
        },
        modifyState(index){
            //读取状态中的squares数组
            var nowList = this.state.squares;
            console.log(nowList);
            //在数组的指定的位置根据状态中的true或false 分别放入 x  或者是 0
            nowList[index] = this.state.xIsNext?"X":"O";
            //修改状态中的数据
            this.setState({squares:nowList,xIsNext:!this.state.xIsNext},()=>{
                //状态修改后的回调函数
                console.log(this.state.squares)
            })
        },
        renderSquare(i) {
            return <Square index={i} list={this.state.squares} myProps={this.modifyState}/>;
        },
        render() {
            return (
                    <div>
                        <div className="status">{status}</div>
                        <div className="board-row">
                        {this.renderSquare(0)}
                        {this.renderSquare(1)}
                        {this.renderSquare(2)}
                    </div>
                    <div className="board-row">
                        {this.renderSquare(3)}
                        {this.renderSquare(4)}
                        {this.renderSquare(5)}
                    </div>
                    <div className="board-row">
                        {this.renderSquare(6)}
                        {this.renderSquare(7)}
                        {this.renderSquare(8)}
                    </div>
                    </div>
    );}})
    class Game extends React.Component {
        render() {
            return (
                    <div className="game">
                        <div className="game-board">
                            <Board />
                        </div>
                        <div className="game-info">
                            <div>{ status }</div>
                            <ol>{/* TODO */}</ol>
                        </div>
                    </div>
        );
        }
    }
    // ========================================
    ReactDOM.render(
    <Game />,
            document.getElementById('container')
    );
    function calculateWinner(squares) {
        const lines = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6],
        ];
        for (let i = 0; i < lines.length; i++) {
            const [a, b, c] = lines[i];
            if (squares[a] &&
                    squares[a] === squares[b] &&
                    squares[a] === squares[c]) {
                return squares[a];
            }
        }
        return null;
    }
</script>

</html>
