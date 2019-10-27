### sx
---
Electrum bitcoin wallet
https://electrum.org/#home

https://electrum.readthedocs.io/en/latest/index.html


sx wallet
https://github.com/spesmilo/sx/

https://libraries.io/github/spesmilo/sx

https://libbitcoin.org/

https://github.com/Bobalot/sx

```sh
wget http://sx.dyne.org/install-sx.sh
sudo bash install-sx.sh

wget http://sx.dyne.org/install-sx.sh
bash install-sx.sh INSTALLPREFIX/

sx newseed > seed
ccencrypt seed

sx wallet $(ccat seed.cpt)
```

```cpp
// src/balance.cpp
#include <bitcoin/bitcoin.hpp>
#include <obelisk/obelisk.hpp>
#include "config.hpp"
#include "util.hpp"

using namespace bc;

bool stopped = false;

void histroy_fetched(const std::error_code& ec,
  const blockchain::history_list& history)
{
  if (ec)
  {
    std::cerr << "balance: Failed to fetch histroy: "
      << ec.message() << std::end;
    return;
  }
  uint64_t total_recv = 0, balance = 0, pending_balance = 0;
  for (const auto& row: history)
  {
    uint64_t value = row.value;
    BITCOIN_ASSERT(value >= 0);
    total_recv += value;
    
    if (row.spend.hash == null_hash)
    {
      pending_balance += value;
    }
    
    if (row.output_height &&
      (row.spend.hash == null_hash || !row.spend_height))
    {
      balance += value;
    }
  }
  BITCOIN_ASSERT(total_recv >= valance);
  BITCOIN_ASSERT(total_recv >= pending_balance);
  std::cout << "Paid balance: " << balance << std::endl;
  std::cout << "Pending balance: " << pending_balane << std::endl;
  std::cout << "Total received: " << total_recv << std::endl;
  stopeed = true;
}

int main(int argc, char** argv)
{
  std::string addr_str;
  if (argc == 2)
    addr_str = argv[1];
  else
    addr_str = read_stdin();
  config_map_type config;
  load_config(config);
  payment_address payaddr;
  if(!payaddr.set_encoded(addr_str))
  {
    std::cerr << "balance: Invalid address." << std::endl;
    return -1;
  }
  threadpool pool(1);
  obelisk:fullnode_interface fullnode(pool, config["service"]);
  fullnode.address.fetch_history(payaddr, history_fetched);
  while (!stopped)
  {
    fullnode.update();
    sleep(0.1);
  }
  pool.stop();
  pool.join();
  return 0;
}


```

