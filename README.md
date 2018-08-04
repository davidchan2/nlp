## bilm

Reference: https://github.com/allenai/bilm-tf

virtualenv venv3

source venv3/bin/activate

pip install tensorflow-gpu==1.2 h5py

pip install -e .


export CUDA_VISIBLE_DEVICES=0


retraining:

	1) vocab_file.txt (not change)
	2) 2.1 train_prefix_dir1/*, 2.2 train_prefix_dir2/*, 2.3 train_prefix_dir3/* .... (replace each retrain)
	3) save_dir (models using the same options.json, n_train_tokens and n_epochs can be changed)
	4) batch_no = n_epochs*n_batches_per_epoch  (where n_batches_per_epoch=n_train_tokens/(batch_size*unroll_steps*n_gpus)
	5) epoch    = batch_no/n_batches_per_epoch + 1


python bin/train_elmo.py \
--train_prefix='data/training_filtered/*' \
--vocab_file data/vocab_filtered.txt \
--save_dir checkpoint


python bin/restart.py \
--train_prefix='data/training_filtered/*' \
--vocab_file data/vocab_filtered.txt \
--save_dir checkpoint


python bin/run_test.py \
--test_prefix='data/heldout_filtered/*' \
--vocab_file data/vocab_filtered.txt \
--save_dir checkpoint
