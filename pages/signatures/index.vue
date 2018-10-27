<template>
  <div>
    <b-row class="mb-3">
      <b-col
        md="3"
        offset-md="9">
        <b-form-select
          v-model="meta.per_page"
          :options="options"/>
      </b-col>
    </b-row>

    <b-table
      :items="signatures"
      :fields="fields"
      striped
      hover>
      <template
        slot="action"
        slot-scope="signature">
        <b-button
          size="sm"
          variant="danger"
          @click.prevent="destroy(signature.item.id)">刪除</b-button>
      </template>
    </b-table>

    <b-pagination
      v-model="meta.current_page"
      :first-text="meta.first_text"
      :prev-text="meta.prev_text"
      :next-text="meta.next_text"
      :last-text="meta.last_text"
      :limit="meta.limit"
      :per-page="meta.per_page"
      :total-rows="meta.total_rows"
      align="center"/>
  </div>
</template>

<script>
	export default {
    layout: 'app',
		data() {
			return {
				url: process.env.baseUrl + '/signatures',
        signatures: [],
				meta: {
					current_page: 1,
					first_text: '第一頁',
					prev_text: '前一頁',
					next_text: '下一頁',
					last_text: '最後頁',
					limit: 7,
					per_page: 10,
					total_rows: 0
				},
				options: [
					{ value: 5, text: 5 },
					{ value: 10, text: 10 },
					{ value: 15, text: 15 }
				],
				fields: [
					{ key: 'id', label: '編號' },
					{ key: 'name', label: '名字' },
					{ key: 'content', label: '內容' },
					{ key: 'action', label: '' }
				]
			}
		},
		watch: {
			'meta.current_page': function() {
				this.fetch()
			},
			'meta.per_page': function() {
				this.fetch()
      }
		},
		created() {
      this.fetch()
		},
    asyncData({ app, env}) {
      return app.$axios.get(`${env.baseUrl}/signatures`)
        .then(({data}) => {
          return {
            signatures: data.data
          }
        })
    },
		methods: {
			fetch() {
				this.$axios.get(`${this.url}?page=${this.meta.current_page}&per_page=${this.meta.per_page}`)
					.then(({data}) => {
						this.signatures = data.data
						this.meta.total_rows = data.meta.total
          })
			},
			destroy(id) {
				if (confirm('確定刪除？')) {
					this.$axios.delete(this.url + '/' + id)
						.then(() => {
							this.data = this.lodash.remove(this.data, function (data) {
								return data.id !== id
								})
							this.fetch(this.url + '?page=' + this.meta.current_page)
						})
				}
			}
		}
	}
</script>
