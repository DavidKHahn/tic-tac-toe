# tic-tac-toe

Build an interactive tic-tac-toe game using React from scratch - coding assessment.

1.  `npx create-react-app tic-tac-toe` (make sure node is up-to-date!)
2.  Delete original source files inside 'src' folder (`cd tic-tac-toe`, `cd src`, `rm -f *`, `cd..`)
3.  Create 'index.css', 'index.js' inside 'src' folder.
4.  In Board’s renderSquare method, change the code to pass a prop called value to the Square:

        class Board extends React.Component {
            renderSquare(i) {
                return <Square value={i} />;
            }
        }

5.  Change Square’s render method to show that value by replacing {/_ TODO _/} with {this.props.value}:

        class Square extends React.Component {
            render() {
                return (
                <button className="square">
                    {this.props.value}
                </button>
                );
            }
        }

6.  Change the button tag that is returned from the Square component’s render() function to this:

        class Square extends React.Component {
            render() {
                return (
                <button className="square" onClick={function() { alert('click'); }}>
                    {this.props.value}
                </button>
                );
            }
        }

7.  Store the current value of the Square in this.state, and change it when the Square is clicked.

First, we’ll add a constructor to the class to initialize the state:

        class Square extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                value: null,
                };
        }

8. Now we’ll change the Square’s render method to display the current state’s value when clicked:

Replace this.props.value with this.state.value inside the <button> tag.
Replace the onClick={...} event handler with onClick={() => this.setState({value: 'X'})}.
Put the className and onClick props on separate lines for better readability.
After these changes, the <button> tag that is returned by the Square’s render method looks like this:

        class Square extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                value: null,
                };
            }

            render() {
                return (
                <button
                    className="square"
                    onClick={() => this.setState({value: 'X'})}
                >
                    {this.state.value}
                </button>
                );
            }
        }

9.  Add a constructor to the Board and set the Board’s initial state to contain an array of 9 nulls corresponding to the 9 squares:

        class Board extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                squares: Array(9).fill(null),
                };
        }

10. Modify the Board’s renderSquare method to read from it:

        renderSquare(i) {
            return <Square value={this.state.squares[i]} />;
        }

11. Pass down a function from the Board to the Square, and we’ll have Square call that function when a square is clicked. We’ll change the renderSquare method in Board to:

        renderSquare(i) {
            return (
            <Square
                value={this.state.squares[i]}
                onClick={() => this.handleClick(i)}
            />
            );
        }

12. Make the following changes to Square:

- Replace this.state.value with this.props.value in Square’s render method
- Replace this.setState() with this.props.onClick() in Square’s render method
- Delete the constructor from Square because Square no longer keeps track of the game’s state

After these changes, the Square component looks like this:

        class Square extends React.Component {
            render() {
                return (
                <button
                    className="square"
                    onClick={() => this.props.onClick()}
                >
                    {this.props.value}
                </button>
                );
            }
        }

13. Add handleClick to the Board class:

        class Board extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                squares: Array(9).fill(null),
                };
        }

        handleClick(i) {
            const squares = this.state.squares.slice();
            squares[i] = 'X';
            this.setState({squares: squares});
        }

14. Replace the Square class with this function:

        function Square(props) {
            return (
                <button className="square" onClick={props.onClick}>
                {props.value}
                </button>
            );
        }

15. Set the first move to be “X” by default. We can set this default by modifying the initial state in our Board constructor:

        class Board extends React.Component {
            constructor(props) {
                super(props);
                this.state = {
                squares: Array(9).fill(null),
                xIsNext: true,
                };
        }

16. xIsNext (a boolean) will be flipped to determine which player goes next and the game’s state will be saved. We’ll update the Board’s handleClick function to flip the value of xIsNext:

        handleClick(i) {
            const squares = this.state.squares.slice();
            squares[i] = this.state.xIsNext ? 'X' : 'O';
            this.setState({
                squares: squares,
                xIsNext: !this.state.xIsNext,
            });
        }

17. Change the “status” text in Board’s render so that it displays which player has the next turn:

            render() {
                const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

18. Copy this helper function and paste it at the end of the file:

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
                if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
                return squares[a];
                }
            }
            return null;
        }

19. We will call calculateWinner(squares) in the Board’s render function to check if a player has won. If a player has won, we can display text such as “Winner: X” or “Winner: O”. We’ll replace the status declaration in Board’s render function with this code:

            render() {
                    const winner = calculateWinner(this.state.squares);
                    let status;
                    if (winner) {
                    status = 'Winner: ' + winner;
                    } else {
                    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
                }

20. We can now change the Board’s handleClick function to return early by ignoring a click if someone has won the game or if a Square is already filled:

               handleClick(i) {
                const squares = this.state.squares.slice();
                if (calculateWinner(squares) || squares[i]) {
                    return;
                }

21. We’ll set up the initial state for the Game component within its constructor:

            class Game extends React.Component {
                constructor(props) {
                    super(props);
                    this.state = {
                    history: [{
                        squares: Array(9).fill(null),
                    }],
                    xIsNext: true,
                };
            }

22.  We now have a single click handler in Board for many Squares, we’ll need to pass the location of each Square into the onClick handler to indicate which Square was clicked:

 - Delete the constructor in Board.
 - Replace this.state.squares[i] with this.props.squares[i] in Board’s renderSquare.
 - Replace this.handleClick(i) with this.props.onClick(i) in Board’s renderSquare.

 23. Update the Game component’s render function to use the most recent history entry to determine and display the game’s status:

        render() {
            const history = this.state.history;
            const current = history[history.length - 1];
            const winner = calculateWinner(current.squares);
            let status;
            if (winner) {
                status = 'Winner: ' + winner;
            } else {
                status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
            }

            return (
            <div className="game">
                <div className="game-board">
                <Board
                    squares={current.squares}
                    onClick={(i) => this.handleClick(i)}
                />
                </div>
                <div className="game-info">
                    <div>{status}</div>

24. The Game component is now rendering the game’s status, we can remove the corresponding code from the Board’s render method. After refactoring, the Board’s render function looks like this:

        render() {
            return (
            <div>
                <div className="board-row">

25. We need to move the handleClick method from the Board component to the Game component. We also need to modify handleClick because the Game component’s state is structured differently. Within the Game’s handleClick method, we concatenate new history entries onto history.

        handleClick(i) {
            const history = this.state.history;
            const current = history[history.length - 1];
            const squares = current.squares.slice();
            if (calculateWinner(squares) || squares[i]) {
                return;
            }
            squares[i] = this.state.xIsNext ? 'X' : 'O';
            this.setState({
            history: history.concat([{
                squares: squares,
            }]),
            xIsNext: !this.state.xIsNext,
            });
        }

26. Map over the history in the Game’s render method:

        render() {
            const history = this.state.history;
            const current = history[history.length - 1];
            const winner = calculateWinner(current.squares);

            const moves = history.map((step, move) => {
            const desc = move ?
                'Go to move #' + move :
                'Go to game start';
            return (
                <li>
                <button onClick={() => this.jumpTo(move)}>{desc}</button>
                </li>
            );
            });

            let status;
            if (winner) {
            status = 'Winner: ' + winner;
            } else {
            status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
            }

            return (
            <div className="game">
                <div className="game-board">
                <Board
                    squares={current.squares}
                    onClick={(i) => this.handleClick(i)}
                />
                </div>
                <div className="game-info">
                <div>{status}</div>
                <ol>{moves}</ol>

