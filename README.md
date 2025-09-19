<h1 align="center">
    ip2asn
  <br>
</h1>

<h4 align="center">A utility to quickly map IP addresses to their respective ASN</h4>

<p align="center">
  <a href="#installation">🏗️ Installation</a>  
  &nbsp;&nbsp;&nbsp;
  <a href="#usage">⛏️ Usage</a>
  &nbsp;&nbsp;&nbsp;
    <a href="#updating-rib-data">🔄 Updating RIB Data</a> 
      &nbsp;&nbsp;&nbsp; 
  <a href="#how-it-works">📖 How It Works</a>
  <br>
</p>


# A modified version of ip2asn, 2025/9/19

- The modified version of ip2asn.py loads `trie_data.json.gz` on the same directory as ip2asn.py
- `trie_data_json.gz` in this directory is generated on 2025/9/19 from the data at archive.routeviews.org.
- `IP2ASN` class is provided

## How to use the IP2ASN class from other scripts

```python
import ip2asn

i2a = ip2asn.IP2ASN()

asn = i2a.lookup("1.1.1.1")

print(asn) # 13335
```




![ip2asn](https://github.com/devanshbatham/ip2asn/blob/main/static/ip2asn.png?raw=true)

# Installation

```sh
git clone https://github.com/devanshbatham/ip2asn
cd ip2asn
sudo chmod +x setup.sh
./setup.sh
```



*Note:* The utility comes pre-packed with trie data, so if you don't wish to update the RIB data, it should work well right off the bat.

# Usage


```sh
(~)>> cat input_ips.txt | ip2asn

74.122.191.97 [AS15211]
74.122.191.99 [AS15211]
151.101.65.49 [AS54113]
162.159.136.66 [AS13335]
75.2.48.152 [AS16509]
76.223.91.57 [AS16509]
80.239.194.78 [AS1299]
99.86.20.101 [AS16509]
99.86.20.118 [AS16509]
```

- With `--json` for JSON output: 

```sh
(~)>> cat input_ips.txt | ip2asn --json

{
  "AS1299": [
    "62.115.144.49",
    "62.115.37.34",
    "62.115.57.70",
    "80.239.194.78"
  ],
  "AS11404": [
    "65.50.201.2"
  ],
  "AS11696": [
    "69.60.198.202"
  ]
}
```



# Updating RIB Data

If you wish to build the trie yourself or want to update the trie data:

1. By default, `trie_data.json.gz` is saved in the `~/.ip2asn/` folder. So, firstly, delete the `~/.ip2asn` folder.
2. Run the `get-rib.sh` script.
3. Build the trie using the `--rib-path` or `--rp` option with the updated RIB data.

```sh
echo "8.8.8.8" | ip2asn --rib-path /path/to/processed_rib.txt
```



# How It Works

The `ip2asn` utility employs a trie data structure to map IP address prefixes to their corresponding Autonomous System Numbers (ASN). Each key in the trie represents an IP address prefix, while the corresponding value denotes the AS number that announces these prefixes. The utility checks this trie to find the longest matching prefix for each IP and retrieves its associated ASN.
