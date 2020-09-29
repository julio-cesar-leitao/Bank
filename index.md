<html>
    <body>
        <div>
			<p> NOVO Bank DApp</p>
			<input type="text" id="amount">
  			<p>Balance: <a id='balance'></a></p>
  			<button id='deposit'>Deposit</button>
  			<button id='withdraw'>withdraw</button>
  			<p>Account Connected: <a id='coinbase'></a></p>
			<p>Actual Block Number: <a id='blockNumber'></a></p>
	    </div>

        <script src="https://cdn.jsdelivr.net/gh/ethereum/web3.js@1.0.0-beta.36/dist/web3.min.js"></script>
        <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" crossorigin="anonymous"></script>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>

        <script>
			var contract;
            $(document).ready(function(){
				// making conection with blockchain
				if (typeof web3 !== 'undefined') {
                    // Use MetaMask's provider
                    web3 = new Web3(web3.currentProvider);

                } else {
			    // Use localhost provider
                    //web3 = new Web3(new Web3.providers.HttpProvider("http://192.168.0.14:8545"));
                }

				/////////////////////////////
				// To see on console
				web3.eth.getAccounts().then(console.log);
				web3.eth.getBlockNumber().then(console.log);
				web3.eth.isMining().then(console.log);

				web3.eth.getCoinbase().then(function(coinbase){
					$('#coinbase').html(coinbase);
				})

				web3.eth.getBlockNumber().then(function(blockNumber){
					$('#blockNumber').html(blockNumber);
				})

				/////////////////////////////
				// Deployed Contract's Adress
				var address = "0xB40dCa2c4b6B84C1131eBDdCf3df6D2f294B0ba8"
        		// Deployed Contract's ABI
				var abi = [
						{
							"constant": false,
							"inputs": [
								{
									"name": "amt",
									"type": "int256"
								}
							],
							"name": "deposit",
							"outputs": [],
							"payable": false,
							"stateMutability": "nonpayable",
							"type": "function"
						},
						{
							"constant": false,
							"inputs": [
								{
									"name": "amt",
									"type": "int256"
								}
							],
							"name": "withdraw",
							"outputs": [],
							"payable": false,
							"stateMutability": "nonpayable",
							"type": "function"
						},
						{
							"inputs": [],
							"payable": false,
							"stateMutability": "nonpayable",
							"type": "constructor"
						},
						{
							"constant": true,
							"inputs": [],
							"name": "getBalance",
							"outputs": [
								{
									"name": "",
									"type": "int256"
								}
							],
							"payable": false,
							"stateMutability": "view",
							"type": "function"
						}
					];
                contract = new web3.eth.Contract(abi, address);
                contract.methods.getBalance().call().then(function(bal)
                {
                    $('#balance').html(bal);
                })
			})

			$('#deposit').click(function()
			{
				M.toast({html:'Transaction received and will be mined!'});
				console.log("Transaction received and will be mined!");
				var amt = 0;
				amt = parseInt($('#amount').val());

				web3.eth.getAccounts().then(function(accounts)
				{
					var acc = accounts[0];
					return contract.methods.deposit(amt).send({from: acc});
				}).then(function(tx)
				{
					console.log(tx);
					if(!alert("Transaction mined at block " + tx.blockNumber + "\nBlockHash = " + tx.blockHash)){window.location.reload();}
				}).catch(function(tx)
				{
					if (tx){
						alert('Some error has occurred, go to console!')
					}
					console.log(tx);
					M.toast({html:tx})
				})
			})

			$('#withdraw').click(function()
			{
				M.toast({html:'Transaction received and will be mined!'});
				console.log("Transaction received and will be mined!");
				var amt = 0;
				amt = parseInt($('#amount').val());

				web3.eth.getAccounts().then(function(accounts)
				{
					var acc = accounts[0];
					return contract.methods.withdraw(amt).send({from: acc});
				}).then(function(tx)
				{
					console.log(tx);
					if(!alert("Transaction mined at block " + tx.blockNumber + "\nBlockHash = " + tx.blockHash)){window.location.reload();}
				}).catch(function(tx)
				{
					if (tx){
						alert('Some error has occurred, go to console!')
					}
					console.log(tx);
					M.toast({html:tx})
				})
			})
        </script>
    </body>
</html>
