# Nolus-Snapshot Instructions

## Stop Service
```sudo systemctl stop nolusd```

### Reset Data
```bash
cd $HOME/.nolus/data/priv_validator_state.json $HOME/.nolus/data/priv_validator_state.json.backup
rm -rf $HOME/.nolus/data
```

#### Installing Snapshot
```bash
curl -L http://snapshot.nolus.nolusination.online/nolus/nolus-snapshot-20230304.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nolus
mv $HOME/.nolus/data/priv_validator_state.json.backup $HOME/.nolus/data/priv_validator_state.json
```

#### Start Service & Check Logs
```
sudo systemctl start nolusd && sudo journalctl -fu nolusd -o cat
```
