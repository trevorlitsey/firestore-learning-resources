# Rest vs. Firestore

A quick table of common queries written in both REST and Firestore. If you don't love this viewing format you can also [view as csv](https://docs.google.com/spreadsheets/d/13y4WdoTO0v573a-aylmiNgiYjKulfVGsljn3TskCMe4/edit#gid=0).

<table class="table table-bordered table-hover table-condensed">
<thead><tr><th title="Field #1">Func</th>
<th title="Field #2">REST API</th>
<th title="Field #3">Firestore API</th>
<th title="Field #4">Notes</th>
</tr></thead>
<tbody><tr>
<td colspan="4"><strong>PLURAL ROUTES</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts</code></td>
<td><code>db.collection(“posts”)</code></td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1</code></td>
<td><code>db.collection(“posts”).doc(‘1’)</code></td>
<td> </td>
</tr>
<tr>
<td>FILTER</td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Use dot(<code>.</code>) to access deep properties</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?title=json-server&amp;author=typicode</code></td>
<td><code>db.collection(“posts”).where(“title”, “==“, “json-server”).where(“author”, “==“, “typicode”)</code></td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?id=1&amp;id=2</code></td>
<td>NA</td>
<td>// ‘or’ queries are not supported</td>
</tr>
<tr>
<td> </td>
<td><code>GET /comments?author.name=typicode</code></td>
<td><code>db.collection(“comments”).where(“author.name”, “==“, “typicode”)</code></td>
<td> </td>
</tr>
<tr>
<td><strong>PAGINATE</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Use <code>_page</code> and optionally <code>_limit</code> to paginate returned data.</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>In the Link header you&#39;ll get first, prev, next and last links.</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts_page=7</code></td>
<td>NA</td>
<td>// page not supported (use startAt and limit)</td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts_page=7&amp;_limit=20</code></td>
<td>NA</td>
<td>// page not supported (use startAt and limit)</td>
</tr>
<tr>
<td><strong>SORT</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Add <code>_sort</code> and <code>_order</code> (ascending order by default)</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?_sort=views&amp;_order=asc</code></td>
<td><code>db.collection(“posts”).orderBy(“views”)</code></td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1/comments?_sort=votes&amp;_order=asc</code></td>
<td><code>db.collection(“posts/1/comments”).orderBy(“votes”)</code></td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>For multiple fields, use the following format:</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?_sort=user,views&amp;_order=desc,asc</code></td>
<td><code>db.collection(“posts”).orderBy(“user”).orderBy(“views”, “desc”)</code></td>
<td> </td>
</tr>
<tr>
<td><strong>SLICE</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Add <code>_start</code> and <code>_end</code> or <code>_limit</code> (an X-Total-Count header is included in the response)</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?_start=20&amp;_end=30</code></td>
<td><code>db.collection(“posts”).startAt(20).limit(10)</code></td>
<td>// not sure if you can use startAt and endAt in the same query</td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1/comments?_start=20&amp;_end=30</code></td>
<td><code>db.collection(“posts/1/comments”).startAt(20).limit(10)</code></td>
<td>// not sure if you can use startAt and endAt in the same query</td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1/comments?_start=20&amp;_limit=10</code></td>
<td><code>db.collection(“posts/1/comments”).startAt(20).limit(10)</code></td>
<td> </td>
</tr>
<tr>
<td><strong>OPERATORS</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Add <code>_gte</code> or <code>_lte</code> for getting a range</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?views_gte=10&amp;views_lte=20</code></td>
<td><code>db.collection(“posts”).where(“views”, “&gt;=“, 10).where(“views”, “&lt;=“, 20)</code></td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Add <code>_ne</code> to exclude a value</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?id_ne=1</code></td>
<td>NA</td>
<td>// != is not supported</td>
</tr>
<tr>
<td colspan="4"><em>Add <code>_like</code> to filter (RegExp supported)</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?title_like=server</code></td>
<td>NA</td>
<td>// RegExp is not supported</td>
</tr>
<tr>
<td><strong>FULL_TEXT SEARCH</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>Add <code>q</code></em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?q=internet</code></td>
<td>NA</td>
<td>// not supported. other solutions: https://firebase.google.com/docs/firestore/solutions/search</td>
</tr>
<tr>
<td><strong>RELATIONSHIPS</strong></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td colspan="4"><em>To include children resources, add <code>_embed</code></em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts?_embed=comments</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1?_embed=comments</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
<tr>
<td colspan="4"><em>To include parent resource, add <code>_expand</code></em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /comments?_expand=post</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
<tr>
<td> </td>
<td><code>GET /comments/1?_expand=post</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
<tr>
<td colspan="4"><em>To get or create nested resources (by default one level, add custom routes for more)</em></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td> </td>
<td><code>GET /posts/1/comments</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
<tr>
<td> </td>
<td><code>POST /posts/1/comments</code></td>
<td>NA</td>
<td>// cross-collection queries are not supported</td>
</tr>
</tbody></table>
