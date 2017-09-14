Step 1: In the Activity / Fragment
    Add this sticky header item decoration to the recycler view and apply the observer to the adapter.

        // Add the sticky headers decoration
        final StickyRecyclerHeadersDecoration headersDecor = new StickyRecyclerHeadersDecoration(mAdapter);
        recyclerView.addItemDecoration(headersDecor);

        mAdapter.registerAdapterDataObserver(new RecyclerView.AdapterDataObserver() {
            @Override
            public void onChanged() {
                headersDecor.invalidateHeaders();
            }
        });

Step 2: In the Adapter-
    Implements adapter with StickyRecyclerHeadersAdapter<RecyclerView.ViewHolder>

        @Override
        public long getHeaderId(int position) {
            try {
                if (position < getItemCount()) {
                    //returning long milliseconds for start of the day
                    return Long.parseLong(new SimpleDateFormat("ddMMyyyy").format(new Date(getItem(position).receivedTime)));
                }
            } catch (NumberFormatException e) {
                e.printStackTrace();
            }
            return -1;
        }

        @Override
        public RecyclerView.ViewHolder onCreateHeaderViewHolder(ViewGroup parent) {
            //return the item layout view
            return new HeaderHolder(InflateChatHeaderBinding.inflate(LayoutInflater.from(parent.getContext()), parent, false));
        }

        @Override
        public void onBindHeaderViewHolder(RecyclerView.ViewHolder holder, int position) {
            if (position < getItemCount()) {
                ((HeaderHolder) holder).bind(getItem(position));
            }
        }

            @Override
            public int getItemCount() {
                return mChatMessageList != null ? mChatMessageList.size() : 0;
            }

            public ChatMessage getItem(int pos) {
                return getItemCount() > pos ? mChatMessageList.get(pos) : null;
            }

        class HeaderHolder extends RecyclerView.ViewHolder {
               private final InflateChatHeaderBinding mHeaderBinding;

               public HeaderHolder(InflateChatHeaderBinding headerBinding) {
                     super(headerBinding.getRoot());
                     mHeaderBinding = headerBinding;
               }

               public void bind(ChatMessage item) {
                      mHeaderBinding.tvChatDate.setText(DateUtils.getFormattedDate(mContext, item.receivedTime));
               }
        }