# tic-tac-toe

build an interactive tic-tac-toe game with React (from scratch) - coding assessment

1. `npx create-react-app tic-tac-toe` (make sure node is up-to-date!)
2. Delete original source files inside 'src' folder (`cd tic-tac-toe`, `cd src`, `rm -f *`, `cd..`)
3. Create 'index.css', 'index.js' inside 'src' folder.
4. In Board’s renderSquare method, change the code to pass a prop called value to the Square:

        class Board extends React.Component {
            renderSquare(i) {
                return <Square value={i} />;
            }
        }
5. Change Square’s render method to show that value by replacing {/* TODO */} with {this.props.value}:

        class Square extends React.Component {
            render() {
                return (
                <button className="square">
                    {this.props.value}
                </button>
                );
            }
        }