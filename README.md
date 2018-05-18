# voting-app
My first attempt to build a decentralized voting app. This is the code for "A Guide to Building Your First Decentralized Application" by Siraj Raval on Youtube

Step 1 - Setting up Environment
Instead of developing the app against the live Ethereum blockchain, we will use an in-memory blockchain (think of it as a blockchain simulator) called testrpc.
npm install ethereumjs-testrpc web3
testrpc creates 10 test accounts to play with automatically. These accounts come preloaded with 100 (fake) ethers.

Step 2 - Creating Voting Smart Contract
We'll use Ethereum's solidity programming language to write our contract
Our contract (think of contract as a class) is called Voting with a constructor which initializes an array of candidates
2 functions, one to return the total votes a candidate has received & another to increment vote count for a candidate.
Deployed contracts are immutable. If any changes, we just make a new one.
Let's install the solidity compiler via npm
npm install solc
After writing our smart contract, we'll use Web3js to deploy our app and interact with it
siraj:~/hello_world_voting$ node
> Web3 = require('web3')
> web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
Then ensure Web3js is initalized and can query all accounts on the blockchain
> web3.eth.accounts
Lastly, compile the contract by loading the code from Voting.sol in to a string variable and compiling it
> code = fs.readFileSync('Voting.sol').toString()
> solc = require('solc')
> compiledCode = solc.compile(code)
Deploy the contract!
dCode.contracts[‘:Voting’].bytecode: bytecode which will be deployed to the blockchain.
compiledCode.contracts[‘:Voting’].interface: interface of the contract (called abi) which tells the contract user what methods are available in the contract.
> abiDefinition = JSON.parse(compiledCode.contracts[':Voting'].interface)
> VotingContract = web3.eth.contract(abiDefinition)
> byteCode = compiledCode.contracts[':Voting'].bytecode
> deployedContract = VotingContract.new(['Rama','Nick','Jose'],{data: byteCode, from: web3.eth.accounts[0], gas: 4700000})
> deployedContract.address
> contractInstance = VotingContract.at(deployedContract.address)
deployedContract.address. When you have to interact with your contract, you need this deployed address and abi definition we talked about earlier.
Step 3 - Interacting with the Contract via the Nodejs Console
> contractInstance.totalVotesFor.call('Rama')
{ [String: '0'] s: 1, e: 0, c: [ 0 ] }
> contractInstance.voteForCandidate('Rama', {from: web3.eth.accounts[0]})
'0xdedc7ae544c3dde74ab5a0b07422c5a51b5240603d31074f5b75c0ebc786bf53'
> contractInstance.voteForCandidate('Rama', {from: web3.eth.accounts[0]})
'0x02c054d238038d68b65d55770fabfca592a5cf6590229ab91bbe7cd72da46de9'
> contractInstance.voteForCandidate('Rama', {from: web3.eth.accounts[0]})
'0x3da069a09577514f2baaa11bc3015a16edf26aad28dffbcd126bde2e71f2b76f'
> contractInstance.totalVotesFor.call('Rama').toLocaleString()
'3'
Step 4 - Creating GUI interface
Let's use our HTML + JS client side skills w00t!