<h1>2.4: Data Persistence</h1>
<div class="document-page-content"><p>To learn about data persistence, you write a simple smart contract that functions as an address book. While this use case is not very practical as a production smart contract, it is a good contract to start with to learn how data persistence works on EOSIO without being distracted by business logic that does not pertain to eosio's <code class="language-text">multi_index</code> functionality.</p>
<h2 id="step-1-create-a-new-directory"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-1-create-a-new-directory" aria-label="step 1 create a new directory permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 1: Create a new directory</h2>
<p>Earlier, you created a contract directory, navigate there now.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token builtin class-name">cd</span> CONTRACTS_DIR</code></pre></div>
<p>Create a new directory for our contract and enter the directory</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token function">mkdir</span> addressbook
<span class="token builtin class-name">cd</span> addressbook</code></pre></div>
<h2 id="step-2-create-and-open-a-new-file"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-2-create-and-open-a-new-file" aria-label="step 2 create and open a new file permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 2: Create and open a new file</h2>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token function">touch</span> addressbook.cpp</code></pre></div>
<p>Open the file in your favorite editor.</p>
<h2 id="step-3-write-an-extended-standard-class-and-include-eosio"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-3-write-an-extended-standard-class-and-include-eosio" aria-label="step 3 write an extended standard class and include eosio permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 3: Write an Extended Standard Class and Include EOSIO</h2>
<p>If you followed the previous tutorial, you created a hello world contract and learned the basics. The code snippet uses a similiar structure with a class named <code class="language-text">addressbook</code>:</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string">&lt;eosio/eosio.hpp&gt;</span></span>

<span class="token keyword">using</span> <span class="token keyword">namespace</span> eosio<span class="token punctuation">;</span>

<span class="token keyword">class</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span><span class="token function">contract</span><span class="token punctuation">(</span><span class="token string">"addressbook"</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">]</span> addressbook <span class="token operator">:</span> <span class="token keyword">public</span> eosio<span class="token operator">::</span>contract <span class="token punctuation">{</span>
  <span class="token keyword">public</span><span class="token operator">:</span>

  <span class="token keyword">private</span><span class="token operator">:</span>

<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<h2 id="step-4-create-the-data-structure-for-the-table"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-4-create-the-data-structure-for-the-table" aria-label="step 4 create the data structure for the table permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 4: Create The Data Structure for the Table</h2>
<p>Before a table can be configured and instantiated, we need to define a struct that represents the data structure of the address book. Since it is an address book, the table will contain people, so create a <code class="language-text">struct</code> called "person"</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token class-name">person</span> <span class="token punctuation">{</span><span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<p>When defining the structure of a multi_index table, you use a unique value as the primary key.</p>
<p>For this contract, use a field called "key" with type <code class="language-text">name</code> based on the user's <code class="language-text">name</code>. This contract has one unique entry per user, so this key will be a consistent and guaranteed unique value.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token class-name">person</span> <span class="token punctuation">{</span>
 name key<span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<p>Since this contract is an address book it should store some relevant details for each entry or <em>person</em></p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token class-name">person</span> <span class="token punctuation">{</span>
 name key<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string first_name<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string last_name<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string street<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string city<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string state<span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<p>Great. The basic data structure is now complete.</p>
<p>Next, define a <code class="language-text">primary_key</code> method. Every <code class="language-text">multi_index</code> struct requires a <em>primary key</em> method. Behind the scenes, this method is used according to the index specification of your <code class="language-text">multi_index</code> instantiation. EOSIO <code class="language-text">multi_index</code> wraps <a href="https://www.boost.org/doc/libs/1_59_0/libs/multi_index/doc/index.html" target="_blank" rel="nofollow noopener noreferrer">boost::multi_index<i class="fa fa-external-link internal-icon" aria-hidden="true"></i></a></p>
<p>Create a method <code class="language-text">primary_key()</code> and return a struct member, in this case, the <code class="language-text">key</code> member as previously discussed.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token class-name">person</span> <span class="token punctuation">{</span>
 name key<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string first_name<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string last_name<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string street<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string city<span class="token punctuation">;</span>
 std<span class="token operator">::</span>string state<span class="token punctuation">;</span>

 <span class="token keyword">uint64_t</span> <span class="token function">primary_key</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">const</span> <span class="token punctuation">{</span> <span class="token keyword">return</span> key<span class="token punctuation">.</span>value<span class="token punctuation">;</span><span class="token punctuation">}</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<div class="custom-block message warning"><div class="custom-block-body"><p>A table's data structure cannot be modified while it has data in it. If you need to make changes to a table's data structure in any way, you first need to remove all its rows</p></div></div>
<h2 id="step-5-configure-the-multi-index-table"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-5-configure-the-multi-index-table" aria-label="step 5 configure the multi index table permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 5: Configure the Multi-Index Table</h2>
<p>Now that the data structure of the table has been defined with a <code class="language-text">struct</code> we need to configure the table. The <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index" target="_self" rel="nofollow noopener noreferrer">eosio::multi_index</a> constructor needs to be named and configured to use the struct we previously defined.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">using</span> address_index <span class="token operator">=</span> eosio<span class="token operator">::</span>multi_index<span class="token operator">&lt;</span><span class="token string">"people"</span>_n<span class="token punctuation">,</span> person<span class="token operator">&gt;</span><span class="token punctuation">;</span></code></pre></div>
<p>With the above <code class="language-text">multi_index</code> configuration there is a table named <strong>people</strong>, that</p>
<ol>
<li>Uses the <code class="language-text">_n</code> operator to define an eosio::name type and uses that to name the table. This table contains a number of different singular "persons", so name the table "people".</li>
<li>Pass in the singular <code class="language-text">person</code> struct defined in the previous step.</li>
<li>Declare this table's type. This type will be used to instantiate this table later.</li>
</ol>
<p>So far, our file should look like this.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string">&lt;eosio/eosio.hpp&gt;</span></span>

<span class="token keyword">using</span> <span class="token keyword">namespace</span> eosio<span class="token punctuation">;</span>

<span class="token keyword">class</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span><span class="token function">contract</span><span class="token punctuation">(</span><span class="token string">"addressbook"</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">]</span> addressbook <span class="token operator">:</span> <span class="token keyword">public</span> eosio<span class="token operator">::</span>contract <span class="token punctuation">{</span>

  <span class="token keyword">public</span><span class="token operator">:</span>

  <span class="token keyword">private</span><span class="token operator">:</span>
    <span class="token keyword">struct</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>table<span class="token punctuation">]</span><span class="token punctuation">]</span> person <span class="token punctuation">{</span>
      name key<span class="token punctuation">;</span>
      std<span class="token operator">::</span>string first_name<span class="token punctuation">;</span>
      std<span class="token operator">::</span>string last_name<span class="token punctuation">;</span>
      std<span class="token operator">::</span>string street<span class="token punctuation">;</span>
      std<span class="token operator">::</span>string city<span class="token punctuation">;</span>
      std<span class="token operator">::</span>string state<span class="token punctuation">;</span>

      <span class="token keyword">uint64_t</span> <span class="token function">primary_key</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">const</span> <span class="token punctuation">{</span> <span class="token keyword">return</span> key<span class="token punctuation">.</span>value<span class="token punctuation">;</span><span class="token punctuation">}</span>
    <span class="token punctuation">}</span><span class="token punctuation">;</span>
    
  <span class="token keyword">using</span> address_index <span class="token operator">=</span> eosio<span class="token operator">::</span>multi_index<span class="token operator">&lt;</span><span class="token string">"people"</span>_n<span class="token punctuation">,</span> person<span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span></code></pre></div>
<h2 id="step-6-the-constructor"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-6-the-constructor" aria-label="step 6 the constructor permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 6: The Constructor</h2>
<p>When working with C++ classes, the first public method you should create is a constructor.</p>
<p>Our constructor will be responsible for initially setting up the contract.</p>
<p>EOSIO contracts extend the <em>contract</em> class. Initialize our parent <em>contract</em> class with the code name of the contract and the receiver. The important parameter here is the <code class="language-text">code</code> parameter which is the account on the blockchain that the contract is being deployed to.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token function">addressbook</span><span class="token punctuation">(</span>name receiver<span class="token punctuation">,</span> name code<span class="token punctuation">,</span> datastream<span class="token operator">&lt;</span><span class="token keyword">const</span> <span class="token keyword">char</span><span class="token operator">*</span><span class="token operator">&gt;</span> ds<span class="token punctuation">)</span><span class="token operator">:</span><span class="token function">contract</span><span class="token punctuation">(</span>receiver<span class="token punctuation">,</span> code<span class="token punctuation">,</span> ds<span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">}</span></code></pre></div>
<h2 id="step-7-adding-a-record-to-the-table"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-7-adding-a-record-to-the-table" aria-label="step 7 adding a record to the table permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 7: Adding a record to the table</h2>
<p>Previously, the primary key of the multi-index table was defined to enforce that this contract will only store one record per user. To make it all work, some rules about the design need to be established.</p>
<ol>
<li>The only account authorized to modify the address book is the user.</li>
<li>The <strong>primary_key</strong> of our table is unique, based on username</li>
<li>For usability, the contract should have the ability to both create and modify a table row with a single action.</li>
</ol>
<p>On a EOSIO blockchain an account name is unique, therefore the <code class="language-text">name</code> type is an ideal candidate as a <strong>primary_key</strong>. Behind the scenes, the <a href="https://developers.eos.io/manuals/eosio.cdt/latest/structeosio_1_1name" target="_self" rel="nofollow noopener noreferrer">name</a> type is an <code class="language-text">uint64_t</code> integer.</p>
<p>Next, define an action for the user to add or update a record. This action will need to accept any values that this action needs to be able to emplace (create) or modify.</p>
<p>For user-experience and interface simplicity, have a single method be responsible for both creation and modification of rows. Because of this behavior, name it "upsert," a combination of "update" and "insert."</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>
  name user<span class="token punctuation">,</span>
  std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span>
  std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span>
  std<span class="token operator">::</span>string street<span class="token punctuation">,</span>
  std<span class="token operator">::</span>string city<span class="token punctuation">,</span>
  std<span class="token operator">::</span>string state
<span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">}</span></code></pre></div>
<p>Earlier, it was mentioned that only the user has control over their own record, as this contract is opt-in. To do this, utilize the <a href="https://developers.eos.io/manuals/eosio.cdt/latest/group__action/#function-require_auth" target="_self" rel="nofollow noopener noreferrer">require_auth</a> method provided by the <code class="language-text">eosio.cdt</code>. This method accepts an <code class="language-text">name</code> type argument and asserts that the account executing the transaction equals the provided value and has the proper permissions to do so.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Previously, a multi_index table was configured, and declared as <code class="language-text">address_index</code>. To instantiate a table, two parameters are required:</p>
<ol>
<li>The first parameter "code", which specifies the owner of this table. As the owner, the account will be charged for storage costs.  Also, only that account can modify or delete the data in this table unless another payer is specified. Here we use the <code class="language-text">get_self()</code> function which will pass the name of this contract.</li>
<li>The second parameter "scope" which ensures the uniqueness of the table in the scope of this contract. In this case, since we only have one table we can use the value from <code class="language-text">get_first_receiver()</code>. The value returned from the <em><code class="language-text">get_first_receiver</code> function is the account name on which this contract is deployed to.</em></li>
</ol>
<p>Note that scopes are used to logically separate tables within a multi-index (see the eosio.token contract multi-index for an example, which scopes the table on the token owner).  Scopes were originally intended to separate table state in order to allow for parallel computation on the individual sub-tables.  However, currently inter-blockchain communication has been prioritized over parallelism.  Because of this, scopes are currently only used to logically separate the tables as in the case of eosio.token.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
  address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Next, query the iterator, setting it to a variable since this iterator will be used several times</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
  address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Security has been established and the table instantiated, great!  Next up, write the code for creating or modifying the table.  </p>
<p>First, detect whether a particular user already exists in the table. To do this, use table's <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index#function-find" target="_self" rel="nofollow noopener noreferrer">find</a> method by passing the <code class="language-text">user</code> parameter. The find method will return an iterator. Use that iterator to test it against the <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index#function-end" target="_self" rel="nofollow noopener noreferrer">end</a> method. The "end" method is an alias for "null".</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
  address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">if</span><span class="token punctuation">(</span> iterator <span class="token operator">==</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    <span class="token comment">//The user isn't in the table</span>
  <span class="token punctuation">}</span>
  <span class="token keyword">else</span> <span class="token punctuation">{</span>
    <span class="token comment">//The user is in the table</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Create a record in the table using the multi_index method <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index/#function-emplace" target="_self" rel="nofollow noopener noreferrer">emplace</a>. This method accepts two arguments, the "payer" of this record who pays the storage usage and a callback function.</p>
<p>The callback function for the emplace method must use a lamba function to create a reference. Inside the body assign the row's values with the ones provided to <code class="language-text">upsert</code>.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
  address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">if</span><span class="token punctuation">(</span> iterator <span class="token operator">==</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    addresses<span class="token punctuation">.</span><span class="token function">emplace</span><span class="token punctuation">(</span>user<span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token operator">&amp;</span><span class="token punctuation">]</span><span class="token punctuation">(</span> <span class="token keyword">auto</span><span class="token operator">&amp;</span> row <span class="token punctuation">)</span> <span class="token punctuation">{</span>
      row<span class="token punctuation">.</span>key <span class="token operator">=</span> user<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>first_name <span class="token operator">=</span> first_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>last_name <span class="token operator">=</span> last_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>street <span class="token operator">=</span> street<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>city <span class="token operator">=</span> city<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>state <span class="token operator">=</span> state<span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
  <span class="token keyword">else</span> <span class="token punctuation">{</span>
    <span class="token comment">//The user is in the table</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Next, handle the modification, or update, case of the "upsert" function. Use the <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index/#function-modify-12" target="_self" rel="nofollow noopener noreferrer">modify</a> method, passing a few arguments:</p>
<ul>
<li>The iterator defined earlier, presently set to the user as declared when calling this action.</li>
<li>The "payer", who will pay for the storage cost of this row, in this case, the user.</li>
<li>The callback function that actually modifies the row.</li>
</ul>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
  address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">if</span><span class="token punctuation">(</span> iterator <span class="token operator">==</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    addresses<span class="token punctuation">.</span><span class="token function">emplace</span><span class="token punctuation">(</span>user<span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token operator">&amp;</span><span class="token punctuation">]</span><span class="token punctuation">(</span> <span class="token keyword">auto</span><span class="token operator">&amp;</span> row <span class="token punctuation">)</span> <span class="token punctuation">{</span>
      row<span class="token punctuation">.</span>key <span class="token operator">=</span> user<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>first_name <span class="token operator">=</span> first_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>last_name <span class="token operator">=</span> last_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>street <span class="token operator">=</span> street<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>city <span class="token operator">=</span> city<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>state <span class="token operator">=</span> state<span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
  <span class="token keyword">else</span> <span class="token punctuation">{</span>
    addresses<span class="token punctuation">.</span><span class="token function">modify</span><span class="token punctuation">(</span>iterator<span class="token punctuation">,</span> user<span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token operator">&amp;</span><span class="token punctuation">]</span><span class="token punctuation">(</span> <span class="token keyword">auto</span><span class="token operator">&amp;</span> row <span class="token punctuation">)</span> <span class="token punctuation">{</span>
      row<span class="token punctuation">.</span>key <span class="token operator">=</span> user<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>first_name <span class="token operator">=</span> first_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>last_name <span class="token operator">=</span> last_name<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>street <span class="token operator">=</span> street<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>city <span class="token operator">=</span> city<span class="token punctuation">;</span>
      row<span class="token punctuation">.</span>state <span class="token operator">=</span> state<span class="token punctuation">;</span>
    <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span></code></pre></div>
<p>The <code class="language-text">addressbook</code> contract now has a functional action that will enable a user to create a row in the table if that record does not yet exist, and modify it if it already exists.</p>
<p>But what if the user wants to remove the record entirely?</p>
<h2 id="step-8-remove-record-from-the-table"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-8-remove-record-from-the-table" aria-label="step 8 remove record from the table permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 8: Remove record from the table</h2>
<p>Similar to the previous steps, create a public method in the <code class="language-text">addressbook</code>, making sure to include the ABI declarations and a <a href="https://developers.eos.io/manuals/eosio.cdt/latest/group__action/#function-require_auth" target="_self" rel="nofollow noopener noreferrer">require_auth</a> that tests against the action's argument <code class="language-text">user</code> to verify only the owner of a record can modify their account.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp">    <span class="token keyword">void</span> <span class="token function">erase</span><span class="token punctuation">(</span>name user<span class="token punctuation">)</span><span class="token punctuation">{</span>
      <span class="token function">require_auth</span><span class="token punctuation">(</span>user<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span></code></pre></div>
<p>Instantiate the table. In <code class="language-text">addressbook</code> each account has only one record. Set <code class="language-text">iterator</code> with <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index/#function-find" target="_self" rel="nofollow noopener noreferrer">find</a></p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
    <span class="token keyword">void</span> <span class="token function">erase</span><span class="token punctuation">(</span>name user<span class="token punctuation">)</span><span class="token punctuation">{</span>
      <span class="token function">require_auth</span><span class="token punctuation">(</span>user<span class="token punctuation">)</span><span class="token punctuation">;</span>
      address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span></code></pre></div>
<p>A contract <em>cannot</em> erase a record that doesn't exist, so check that the record indeed exists before proceeding.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
    <span class="token keyword">void</span> <span class="token function">erase</span><span class="token punctuation">(</span>name user<span class="token punctuation">)</span><span class="token punctuation">{</span>
      <span class="token function">require_auth</span><span class="token punctuation">(</span>user<span class="token punctuation">)</span><span class="token punctuation">;</span>
      address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token function">check</span><span class="token punctuation">(</span>iterator <span class="token operator">!=</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">"Record does not exist"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span></code></pre></div>
<p>Finally, call the <a href="https://developers.eos.io/manuals/eosio.cdt/latest/classeosio_1_1multi__index/#function-erase-12" target="_self" rel="nofollow noopener noreferrer">erase</a> method, to erase the iterator. Once the row is erased, the storage space will be free up for the original payer.</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
  <span class="token keyword">void</span> <span class="token function">erase</span><span class="token punctuation">(</span>name user<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token function">require_auth</span><span class="token punctuation">(</span>user<span class="token punctuation">)</span><span class="token punctuation">;</span>
    address_index <span class="token function">addresses</span><span class="token punctuation">(</span><span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">check</span><span class="token punctuation">(</span>iterator <span class="token operator">!=</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">"Record does not exist"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    addresses<span class="token punctuation">.</span><span class="token function">erase</span><span class="token punctuation">(</span>iterator<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span></code></pre></div>
<p>The contract is now mostly complete. Users can create, modify and erase records. However, the contract is not quite ready to be compiled.</p>
<h2 id="step-9-preparing-for-the-abi"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-9-preparing-for-the-abi" aria-label="step 9 preparing for the abi permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 9: Preparing for the ABI</h2>
<h3 id="91-abi-action-declarations"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#91-abi-action-declarations" aria-label="91 abi action declarations permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>9.1 ABI Action Declarations</h3>
<p><a href="https://developers.eos.io/manuals/eosio.cdt/latest" target="_self" rel="nofollow noopener noreferrer">eosio.cdt</a> includes an ABI Generator, but for it to work will require some declarations.</p>
<p>Above both the <code class="language-text">upsert</code> and <code class="language-text">erase</code> functions add the following C++11 declaration:</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>action<span class="token punctuation">]</span><span class="token punctuation">]</span></code></pre></div>
<p>The above declaration will extract the arguments of the action and create necessary ABI <em>struct</em> descriptions in the generated ABI file.</p>
<h3 id="92-abi-table-declarations"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#92-abi-table-declarations" aria-label="92 abi table declarations permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>9.2 ABI Table Declarations</h3>
<p>Add an ABI declaration to the table. Modify the following line defined in the private region of your contract:</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token class-name">person</span> <span class="token punctuation">{</span></code></pre></div>
<p>To this:</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token keyword">struct</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>table<span class="token punctuation">]</span><span class="token punctuation">]</span> person <span class="token punctuation">{</span></code></pre></div>
<p>The <code class="language-text">[[eosio.table]]</code> declaration will add the necessary descriptions to the ABI file.</p>
<p>Now our contract is ready to be compiled.</p>
<p>Below is the final state of our <code class="language-text">addressbook</code> contract:</p>
<div class="gatsby-highlight" data-language="cpp"><pre class="language-cpp"><code class="language-cpp"><span class="token macro property"><span class="token directive-hash">#</span><span class="token directive keyword">include</span> <span class="token string">&lt;eosio/eosio.hpp&gt;</span></span>

<span class="token keyword">using</span> <span class="token keyword">namespace</span> eosio<span class="token punctuation">;</span>

<span class="token keyword">class</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span><span class="token function">contract</span><span class="token punctuation">(</span><span class="token string">"addressbook"</span><span class="token punctuation">)</span><span class="token punctuation">]</span><span class="token punctuation">]</span> addressbook <span class="token operator">:</span> <span class="token keyword">public</span> eosio<span class="token operator">::</span>contract <span class="token punctuation">{</span>

<span class="token keyword">public</span><span class="token operator">:</span>

  <span class="token function">addressbook</span><span class="token punctuation">(</span>name receiver<span class="token punctuation">,</span> name code<span class="token punctuation">,</span>  datastream<span class="token operator">&lt;</span><span class="token keyword">const</span> <span class="token keyword">char</span><span class="token operator">*</span><span class="token operator">&gt;</span> ds<span class="token punctuation">)</span><span class="token operator">:</span> <span class="token function">contract</span><span class="token punctuation">(</span>receiver<span class="token punctuation">,</span> code<span class="token punctuation">,</span> ds<span class="token punctuation">)</span> <span class="token punctuation">{</span><span class="token punctuation">}</span>

  <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>action<span class="token punctuation">]</span><span class="token punctuation">]</span>
  <span class="token keyword">void</span> <span class="token function">upsert</span><span class="token punctuation">(</span>name user<span class="token punctuation">,</span> std<span class="token operator">::</span>string first_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string last_name<span class="token punctuation">,</span> std<span class="token operator">::</span>string street<span class="token punctuation">,</span> std<span class="token operator">::</span>string city<span class="token punctuation">,</span> std<span class="token operator">::</span>string state<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token function">require_auth</span><span class="token punctuation">(</span> user <span class="token punctuation">)</span><span class="token punctuation">;</span>
    address_index <span class="token function">addresses</span><span class="token punctuation">(</span> <span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value <span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">if</span><span class="token punctuation">(</span> iterator <span class="token operator">==</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">)</span>
    <span class="token punctuation">{</span>
      addresses<span class="token punctuation">.</span><span class="token function">emplace</span><span class="token punctuation">(</span>user<span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token operator">&amp;</span><span class="token punctuation">]</span><span class="token punctuation">(</span> <span class="token keyword">auto</span><span class="token operator">&amp;</span> row <span class="token punctuation">)</span> <span class="token punctuation">{</span>
       row<span class="token punctuation">.</span>key <span class="token operator">=</span> user<span class="token punctuation">;</span>
       row<span class="token punctuation">.</span>first_name <span class="token operator">=</span> first_name<span class="token punctuation">;</span>
       row<span class="token punctuation">.</span>last_name <span class="token operator">=</span> last_name<span class="token punctuation">;</span>
       row<span class="token punctuation">.</span>street <span class="token operator">=</span> street<span class="token punctuation">;</span>
       row<span class="token punctuation">.</span>city <span class="token operator">=</span> city<span class="token punctuation">;</span>
       row<span class="token punctuation">.</span>state <span class="token operator">=</span> state<span class="token punctuation">;</span>
      <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
    <span class="token keyword">else</span> <span class="token punctuation">{</span>
      addresses<span class="token punctuation">.</span><span class="token function">modify</span><span class="token punctuation">(</span>iterator<span class="token punctuation">,</span> user<span class="token punctuation">,</span> <span class="token punctuation">[</span><span class="token operator">&amp;</span><span class="token punctuation">]</span><span class="token punctuation">(</span> <span class="token keyword">auto</span><span class="token operator">&amp;</span> row <span class="token punctuation">)</span> <span class="token punctuation">{</span>
        row<span class="token punctuation">.</span>key <span class="token operator">=</span> user<span class="token punctuation">;</span>
        row<span class="token punctuation">.</span>first_name <span class="token operator">=</span> first_name<span class="token punctuation">;</span>
        row<span class="token punctuation">.</span>last_name <span class="token operator">=</span> last_name<span class="token punctuation">;</span>
        row<span class="token punctuation">.</span>street <span class="token operator">=</span> street<span class="token punctuation">;</span>
        row<span class="token punctuation">.</span>city <span class="token operator">=</span> city<span class="token punctuation">;</span>
        row<span class="token punctuation">.</span>state <span class="token operator">=</span> state<span class="token punctuation">;</span>
      <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>

  <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>action<span class="token punctuation">]</span><span class="token punctuation">]</span>
  <span class="token keyword">void</span> <span class="token function">erase</span><span class="token punctuation">(</span>name user<span class="token punctuation">)</span> <span class="token punctuation">{</span>
    <span class="token function">require_auth</span><span class="token punctuation">(</span>user<span class="token punctuation">)</span><span class="token punctuation">;</span>

    address_index <span class="token function">addresses</span><span class="token punctuation">(</span> <span class="token function">get_self</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token function">get_first_receiver</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>

    <span class="token keyword">auto</span> iterator <span class="token operator">=</span> addresses<span class="token punctuation">.</span><span class="token function">find</span><span class="token punctuation">(</span>user<span class="token punctuation">.</span>value<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">check</span><span class="token punctuation">(</span>iterator <span class="token operator">!=</span> addresses<span class="token punctuation">.</span><span class="token function">end</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token string">"Record does not exist"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    addresses<span class="token punctuation">.</span><span class="token function">erase</span><span class="token punctuation">(</span>iterator<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

<span class="token keyword">private</span><span class="token operator">:</span>
  <span class="token keyword">struct</span> <span class="token punctuation">[</span><span class="token punctuation">[</span>eosio<span class="token operator">::</span>table<span class="token punctuation">]</span><span class="token punctuation">]</span> person <span class="token punctuation">{</span>
    name key<span class="token punctuation">;</span>
    std<span class="token operator">::</span>string first_name<span class="token punctuation">;</span>
    std<span class="token operator">::</span>string last_name<span class="token punctuation">;</span>
    std<span class="token operator">::</span>string street<span class="token punctuation">;</span>
    std<span class="token operator">::</span>string city<span class="token punctuation">;</span>
    std<span class="token operator">::</span>string state<span class="token punctuation">;</span>
    <span class="token keyword">uint64_t</span> <span class="token function">primary_key</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token keyword">const</span> <span class="token punctuation">{</span> <span class="token keyword">return</span> key<span class="token punctuation">.</span>value<span class="token punctuation">;</span> <span class="token punctuation">}</span>
  <span class="token punctuation">}</span><span class="token punctuation">;</span>
  <span class="token keyword">using</span> address_index <span class="token operator">=</span> eosio<span class="token operator">::</span>multi_index<span class="token operator">&lt;</span><span class="token string">"people"</span>_n<span class="token punctuation">,</span> person<span class="token operator">&gt;</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">;</span>    </code></pre></div>
<h2 id="step-10-prepare-the-ricardian-contract-optional"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-10-prepare-the-ricardian-contract-optional" aria-label="step 10 prepare the ricardian contract optional permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 10 Prepare the Ricardian Contract [Optional]</h2>
<p>Contracts compiled without a Ricardian contract will generate a compiler warning for each action missing an entry in the Ricardian clause.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">Warning, action <span class="token operator">&lt;</span>upsert<span class="token operator">&gt;</span> does not have a ricardian contract
Warning, action <span class="token operator">&lt;</span>erase<span class="token operator">&gt;</span> does not have a ricardian contract</code></pre></div>
<p>To define Ricardian contracts for this smart contract, create a new file called addressbook.contracts.md. Notice that the name of the Ricardian contracts must match the name of the smart contract.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token function">touch</span> addressbook.contracts.md</code></pre></div>
<p>Add Ricardian Contract definitions to this file:</p>
<div class="gatsby-highlight" data-language="yaml"><pre class="language-yaml"><code class="language-yaml">&lt;h1 class="contract"<span class="token punctuation">&gt;</span>upsert&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.2
<span class="token key atrule">title</span><span class="token punctuation">:</span> Upsert
<span class="token key atrule">summary</span><span class="token punctuation">:</span> This action will either insert or update an entry in the address book. If an entry exists with the same name as the specified user parameter<span class="token punctuation">,</span> the record is updated with the first_name<span class="token punctuation">,</span> last_name<span class="token punctuation">,</span> street<span class="token punctuation">,</span> city<span class="token punctuation">,</span> and state parameters. If a record does not exist<span class="token punctuation">,</span> a new record is created. The data is stored in the multi index table. The ram costs are paid by the smart contract.
<span class="token key atrule">icon</span><span class="token punctuation">:</span>

&lt;h1 class="contract"<span class="token punctuation">&gt;</span>erase&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.2
<span class="token key atrule">title</span><span class="token punctuation">:</span> Erase
<span class="token key atrule">summary</span><span class="token punctuation">:</span> This action will remove an entry from the address book if an entry in the multi index table exists with the specified name.
icon<span class="token punctuation">:</span></code></pre></div>
<h2 id="step-11-prepare-the-ricardian-clauses-optional"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-11-prepare-the-ricardian-clauses-optional" aria-label="step 11 prepare the ricardian clauses optional permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 11 Prepare the Ricardian Clauses [Optional]</h2>
<p>To define Ricardian clauses for this smart contract create and open a new file called addressbook.clauses.md. Notice again that the name of the Ricardian clauses must match the name of the smart contract.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token function">touch</span> addressbook.clauses.md</code></pre></div>
<p>Add Ricardian clause definitions to this file:</p>
<div class="gatsby-highlight" data-language="yaml"><pre class="language-yaml"><code class="language-yaml">&lt;h1 class="clause"<span class="token punctuation">&gt;</span>Data Storage&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.1
<span class="token key atrule">title</span><span class="token punctuation">:</span> General Data Storage
<span class="token key atrule">summary</span><span class="token punctuation">:</span> This smart contract will store data added by the user. The user consents to the storage of this data by signing the transaction.
<span class="token key atrule">icon</span><span class="token punctuation">:</span>


&lt;h1 class="clause"<span class="token punctuation">&gt;</span>Data Usage&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.1
<span class="token key atrule">title</span><span class="token punctuation">:</span> General Data Use
<span class="token key atrule">summary</span><span class="token punctuation">:</span> This smart contract will store user data. The smart contract will not use the stored data for any purpose outside store and delete.
<span class="token key atrule">icon</span><span class="token punctuation">:</span>

&lt;h1 class="clause"<span class="token punctuation">&gt;</span>Data Ownership&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.1
<span class="token key atrule">title</span><span class="token punctuation">:</span> Data Ownership
<span class="token key atrule">summary</span><span class="token punctuation">:</span> The user of this smart contract verifies that the data is owned by the smart contract<span class="token punctuation">,</span> and that the smart contract can use the data in accordance to the terms defined in the Ricardian Contract.
<span class="token key atrule">icon</span><span class="token punctuation">:</span>

&lt;h1 class="clause"<span class="token punctuation">&gt;</span>Data Distirbution&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.1
<span class="token key atrule">title</span><span class="token punctuation">:</span> Data Distirbution
<span class="token key atrule">summary</span><span class="token punctuation">:</span> The smart contract promises to not actively share or distribute the address data. The user of the smart contract understands that data stored in a multi index table is not private data and can be accessed by any user of the blockchain.  
<span class="token key atrule">icon</span><span class="token punctuation">:</span>


&lt;h1 class="clause"<span class="token punctuation">&gt;</span>Data Future&lt;/h1<span class="token punctuation">&gt;</span>
<span class="token punctuation">---</span>
<span class="token key atrule">spec-version</span><span class="token punctuation">:</span> 0.0.1
<span class="token key atrule">title</span><span class="token punctuation">:</span> Data Future
<span class="token key atrule">summary</span><span class="token punctuation">:</span> The smart contract promises to only use the data in accordance of the terms defined in the Ricardian Contract<span class="token punctuation">,</span> now and at all future dates.
icon<span class="token punctuation">:</span></code></pre></div>
<h2 id="step-12-compile-the-contract"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-12-compile-the-contract" aria-label="step 12 compile the contract permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 12: Compile the Contract</h2>
<p>Execute the following command from your terminal.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">eosio-cpp addressbook.cpp -o addressbook.wasm</code></pre></div>
<p>If you created a Ricardian contract and Ricardian clauses, the definitions will appear in the .abi file. An example for the addressbook.cpp, built including the contract and clause definitions described above is shown below.</p>
<div class="gatsby-highlight" data-language="json"><pre class="language-json"><code class="language-json"><span class="token punctuation">{</span>
    <span class="token property">"____comment"</span><span class="token operator">:</span> <span class="token string">"This file was generated with eosio-abigen. DO NOT EDIT "</span><span class="token punctuation">,</span>
    <span class="token property">"version"</span><span class="token operator">:</span> <span class="token string">"eosio::abi/1.1"</span><span class="token punctuation">,</span>
    <span class="token property">"types"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"structs"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"erase"</span><span class="token punctuation">,</span>
            <span class="token property">"base"</span><span class="token operator">:</span> <span class="token string">""</span><span class="token punctuation">,</span>
            <span class="token property">"fields"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"user"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"name"</span>
                <span class="token punctuation">}</span>
            <span class="token punctuation">]</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"person"</span><span class="token punctuation">,</span>
            <span class="token property">"base"</span><span class="token operator">:</span> <span class="token string">""</span><span class="token punctuation">,</span>
            <span class="token property">"fields"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"key"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"name"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"first_name"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"last_name"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"street"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"city"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"state"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span>
            <span class="token punctuation">]</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"upsert"</span><span class="token punctuation">,</span>
            <span class="token property">"base"</span><span class="token operator">:</span> <span class="token string">""</span><span class="token punctuation">,</span>
            <span class="token property">"fields"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"user"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"name"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"first_name"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"last_name"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"street"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"city"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span><span class="token punctuation">,</span>
                <span class="token punctuation">{</span>
                    <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"state"</span><span class="token punctuation">,</span>
                    <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"string"</span>
                <span class="token punctuation">}</span>
            <span class="token punctuation">]</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"actions"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"erase"</span><span class="token punctuation">,</span>
            <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"erase"</span><span class="token punctuation">,</span>
            <span class="token property">"ricardian_contract"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.2\ntitle: Erase\nsummary: his action will remove an entry from the address book if an entry exists with the same name \nicon:"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"upsert"</span><span class="token punctuation">,</span>
            <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"upsert"</span><span class="token punctuation">,</span>
            <span class="token property">"ricardian_contract"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.2\ntitle: Upsert\nsummary: This action will either insert or update an entry in the address book. If an entry exists with the same name as the user parameter the record is updated with the first_name, last_name, street, city and state parameters. If a record does not exist a new record is created. The data is stored in the multi index table. The ram costs are paid by the smart contract.\nicon:"</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"tables"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"name"</span><span class="token operator">:</span> <span class="token string">"people"</span><span class="token punctuation">,</span>
            <span class="token property">"type"</span><span class="token operator">:</span> <span class="token string">"person"</span><span class="token punctuation">,</span>
            <span class="token property">"index_type"</span><span class="token operator">:</span> <span class="token string">"i64"</span><span class="token punctuation">,</span>
            <span class="token property">"key_names"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
            <span class="token property">"key_types"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"ricardian_clauses"</span><span class="token operator">:</span> <span class="token punctuation">[</span>
        <span class="token punctuation">{</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"Data Storage"</span><span class="token punctuation">,</span>
            <span class="token property">"body"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.1\ntitle: General data Storage\nsummary: This smart contract will store data added by the user. The user verifies they are happy for this data to be stored.\nicon:"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"Data Usage"</span><span class="token punctuation">,</span>
            <span class="token property">"body"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.1\ntitle: General data Use\nsummary: This smart contract will store user data. The smart contract will not use the stored data for any purpose outside store and delete \nicon:"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"Data Ownership"</span><span class="token punctuation">,</span>
            <span class="token property">"body"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.1\ntitle: Data Ownership\nsummary: The user of this smart contract verifies that the data is owned by the smart contract, and that the smart contract can use the data in accordance to the terms defined in the Ricardian Contract \nicon:"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"Data Distirbution"</span><span class="token punctuation">,</span>
            <span class="token property">"body"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.1\ntitle: Data Ownership\nsummary: The smart contract promises to not actively share or distribute the address data. The user of the smart contract understands that data stored in a multi index table is not private data and can be accessed by any user of the blockchain.  \nicon:"</span>
        <span class="token punctuation">}</span><span class="token punctuation">,</span>
        <span class="token punctuation">{</span>
            <span class="token property">"id"</span><span class="token operator">:</span> <span class="token string">"Data Future"</span><span class="token punctuation">,</span>
            <span class="token property">"body"</span><span class="token operator">:</span> <span class="token string">"---\nspec-version: 0.0.1\ntitle: Data Ownership\nsummary: The smart contract promises to only use the data in accordance to the terms defined in the Ricardian Contract, now and at all future dates. \nicon:"</span>
        <span class="token punctuation">}</span>
    <span class="token punctuation">]</span><span class="token punctuation">,</span>
    <span class="token property">"variants"</span><span class="token operator">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span>
<span class="token punctuation">}</span></code></pre></div>
<h2 id="step-13-deploy-the-contract"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-13-deploy-the-contract" aria-label="step 13 deploy the contract permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 13: Deploy the Contract</h2>
<p>Create an account for the contract, execute the following shell command</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">cleos create account eosio addressbook YOUR_PUBLIC_KEY -p eosio@active</code></pre></div>
<p>Deploy the <code class="language-text">addressbook</code> contract</p>
<div class="gatsby-highlight" data-language="text"><pre class="language-text"><code class="language-text">cleos set contract addressbook CONTRACTS_DIR/addressbook -p addressbook@active</code></pre></div>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">5f78f9aea400783342b41a989b1b4821ffca006cd76ead38ebdf97428559daa0  <span class="token number">5152</span> bytes  <span class="token number">727</span> us
<span class="token comment">#         eosio &lt;= eosio::setcode               {"account":"addressbook","vmtype":0,"vmversion":0,"code":"0061736d010000000191011760077f7e7f7f7f7f7f...</span>
<span class="token comment">#         eosio &lt;= eosio::setabi                {"account":"addressbook","abi":"0e656f73696f3a3a6162692f312e30010c6163636f756e745f6e616d65046e616d65...</span>
warning: transaction executed locally, but may not be confirmed by the network yet    <span class="token punctuation">]</span></code></pre></div>
<h2 id="step-14-test-the-contract"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#step-14-test-the-contract" aria-label="step 14 test the contract permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Step 14: Test the Contract</h2>
<p>Add a row to the table</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">cleos push action addressbook upsert <span class="token string">'["alice", "alice", "liddell", "123 drink me way", "wonderland", "amsterdam"]'</span> -p alice@active</code></pre></div>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">executed transaction: 003f787824c7823b2cc8210f34daed592c2cfa66cbbfd4b904308b0dfeb0c811  <span class="token number">152</span> bytes  <span class="token number">692</span> us
<span class="token comment">#   addressbook &lt;= addressbook::upsert          {"user":"alice","first_name":"alice","last_name":"liddell","street":"123 drink me way","city":"wonde...</span></code></pre></div>
<p>Check that <strong>alice</strong> cannot add records for another user.</p>
<div class="gatsby-highlight" data-language="text"><pre class="language-text"><code class="language-text">cleos push action addressbook upsert '["bob", "bob", "is a loser", "doesnt exist", "somewhere", "someplace"]' -p alice@active</code></pre></div>
<p>As expected, the <code class="language-text">require_auth</code> in our contract prevented alice from creating/modifying another user's row.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">Error <span class="token number">3090004</span>: Missing required authority
Ensure that you have the related authority inside your transaction<span class="token operator">!</span><span class="token punctuation">;</span>
If you are currently using <span class="token string">'cleos push action'</span> command, try to <span class="token function">add</span> the relevant authority using -p option.
Error Details:
missing authority of bob</code></pre></div>
<p>Retrieve alice's record.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">cleos get table addressbook addressbook people --lower alice --limit <span class="token number">1</span></code></pre></div>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token punctuation">{</span>
  <span class="token string">"rows"</span><span class="token builtin class-name">:</span> <span class="token punctuation">[</span><span class="token punctuation">{</span>
      <span class="token string">"key"</span><span class="token builtin class-name">:</span> <span class="token string">"alice"</span>,
      <span class="token string">"first_name"</span><span class="token builtin class-name">:</span> <span class="token string">"alice"</span>,
      <span class="token string">"last_name"</span><span class="token builtin class-name">:</span> <span class="token string">"liddell"</span>,
      <span class="token string">"street"</span><span class="token builtin class-name">:</span> <span class="token string">"123 drink me way"</span>,
      <span class="token string">"city"</span><span class="token builtin class-name">:</span> <span class="token string">"wonderland"</span>,
      <span class="token string">"state"</span><span class="token builtin class-name">:</span> <span class="token string">"amsterdam"</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">]</span>,
  <span class="token string">"more"</span><span class="token builtin class-name">:</span> false,
  <span class="token string">"next_key"</span><span class="token builtin class-name">:</span> <span class="token string">""</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Test to see that <strong>alice</strong> can remove the record.</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">cleos push action addressbook erase <span class="token string">'["alice"]'</span> -p alice@active</code></pre></div>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">executed transaction: 0a690e21f259bb4e37242cdb57d768a49a95e39a83749a02bced652ac4b3f4ed  <span class="token number">104</span> bytes  <span class="token number">1623</span> us
<span class="token comment">#   addressbook &lt;= addressbook::erase           {"user":"alice"}</span>
warning: transaction executed locally, but may not be confirmed by the network yet    <span class="token punctuation">]</span></code></pre></div>
<p>Check that the record was removed:</p>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell">cleos get table addressbook addressbook people --lower alice --limit <span class="token number">1</span></code></pre></div>
<div class="gatsby-highlight" data-language="shell"><pre class="language-shell"><code class="language-shell"><span class="token punctuation">{</span>
  <span class="token string">"rows"</span><span class="token builtin class-name">:</span> <span class="token punctuation">[</span><span class="token punctuation">]</span>,
  <span class="token string">"more"</span><span class="token builtin class-name">:</span> false，
  <span class="token string">"next_key"</span><span class="token builtin class-name">:</span> <span class="token string">""</span>
<span class="token punctuation">}</span></code></pre></div>
<p>Looking good!</p>
<h2 id="wrapping-up"><a href="https://developers.eos.io/welcome/latest/getting-started/smart-contract-development/data-persistence/#wrapping-up" aria-label="wrapping up permalink" class="anchor" target="_self"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Wrapping Up</h2>
<p>You've learned how to configure tables, instantiate tables, create new rows, modify existing rows and work with iterators. You've learned how to test against an empty iterator result. Congrats!</p>
