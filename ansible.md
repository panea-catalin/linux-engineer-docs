<section id="building-an-inventory">
<span id="get-started-inventory"></span><h1>Building an inventory</h1>
<p>Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command.</p>
<p>To complete the following steps, you will need the IP address or fully qualified domain name (FQDN) of at least one host system. For demonstration purposes, the host could be running locally in a container or a virtual machine. You must also ensure that your public SSH key is added to the <code class="docutils literal notranslate"><span class="pre">authorized_keys</span></code> file on each host.</p>
<p>Continue getting started with Ansible and build an inventory as follows:</p>
<ol class="arabic">
<li><p>Create a file named <code class="docutils literal notranslate"><span class="pre">inventory.ini</span></code> in the <code class="docutils literal notranslate"><span class="pre">ansible_quickstart</span></code> directory that you created in the <a class="reference internal" href="get_started_ansible.html#get-started-ansible"><span class="std std-ref">preceding step</span></a>.</p></li>
<li><p>Add a new <code class="docutils literal notranslate"><span class="pre">[myhosts]</span></code> group to the <code class="docutils literal notranslate"><span class="pre">inventory.ini</span></code> file and specify the IP address or fully qualified domain name (FQDN) of each host system.</p>
<div class="highlight-ini notranslate"><div class="highlight"><pre><span></span><span class="k">[myhosts]</span>
<span class="na">192.0.2.50</span>
<span class="na">192.0.2.51</span>
<span class="na">192.0.2.52</span>
</pre></div>
</div>
</li>
<li><p>Verify your inventory.</p>
<div class="highlight-bash notranslate"><div class="highlight"><pre><span></span>ansible-inventory<span class="w"> </span>-i<span class="w"> </span>inventory.ini<span class="w"> </span>--list
</pre></div>
</div>
</li>
<li><p>Ping the <code class="docutils literal notranslate"><span class="pre">myhosts</span></code> group in your inventory.</p>
<div class="highlight-bash notranslate"><div class="highlight"><pre><span></span>ansible<span class="w"> </span>myhosts<span class="w"> </span>-m<span class="w"> </span>ping<span class="w"> </span>-i<span class="w"> </span>inventory.ini
</pre></div>
</div>
<div class="admonition note">
<p class="admonition-title">Note</p>
<p>Pass the <code class="docutils literal notranslate"><span class="pre">-u</span></code> option with the <code class="docutils literal notranslate"><span class="pre">ansible</span></code> command if the username is different on the control node and the managed node(s).</p>
</div>
<div class="highlight-text notranslate"><div class="highlight"><pre><span></span>192.0.2.50 | SUCCESS =&gt; {
    &quot;ansible_facts&quot;: {
        &quot;discovered_interpreter_python&quot;: &quot;/usr/bin/python3&quot;
    },
    &quot;changed&quot;: false,
    &quot;ping&quot;: &quot;pong&quot;
}
192.0.2.51 | SUCCESS =&gt; {
    &quot;ansible_facts&quot;: {
        &quot;discovered_interpreter_python&quot;: &quot;/usr/bin/python3&quot;
    },
    &quot;changed&quot;: false,
    &quot;ping&quot;: &quot;pong&quot;
}
192.0.2.52 | SUCCESS =&gt; {
    &quot;ansible_facts&quot;: {
        &quot;discovered_interpreter_python&quot;: &quot;/usr/bin/python3&quot;
    },
    &quot;changed&quot;: false,
    &quot;ping&quot;: &quot;pong&quot;
}
</pre></div>
</div>
</li>
</ol>
<p>Congratulations, you have successfully built an inventory. Continue getting started with Ansible by <a class="reference internal" href="get_started_playbook.html#get-started-playbook"><span class="std std-ref">creating a playbook</span></a>.</p>
</section>

<section id="inventories-in-ini-or-yaml-format">
<h2>Inventories in INI or YAML format</h2>
<p>You can create inventories in either <code class="docutils literal notranslate"><span class="pre">INI</span></code> files or in <code class="docutils literal notranslate"><span class="pre">YAML</span></code>. In most cases, such as the example in the preceding steps, <code class="docutils literal notranslate"><span class="pre">INI</span></code> files are straightforward and easy to read for a small number of managed nodes.</p>
<p>Creating an inventory in <code class="docutils literal notranslate"><span class="pre">YAML</span></code> format becomes a sensible option as the number of managed nodes increases. For example, the following is an equivalent of the <code class="docutils literal notranslate"><span class="pre">inventory.ini</span></code> that declares unique names for managed nodes and uses the <code class="docutils literal notranslate"><span class="pre">ansible_host</span></code> field:</p>
<div class="highlight-yaml notranslate"><div class="highlight"><pre><span></span><span class="nt">myhosts</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">my_host_01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.50</span>
<span class="w">    </span><span class="nt">my_host_02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.51</span>
<span class="w">    </span><span class="nt">my_host_03</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.52</span>
</pre></div>
</div>
</section>

<section id="tips-for-building-inventories">
<h2>Tips for building inventories</h2>
<ul>
<li><p>Ensure that group names are meaningful and unique. Group names are also case sensitive.</p></li>
<li><p>Avoid spaces, hyphens, and preceding numbers (use <code class="docutils literal notranslate"><span class="pre">floor_19</span></code>, not <code class="docutils literal notranslate"><span class="pre">19th_floor</span></code>) in group names.</p></li>
<li><p>Group hosts in your inventory logically according to their <strong>What</strong>, <strong>Where</strong>, and <strong>When</strong>.</p>
<dl class="simple">
<dt>What</dt><dd><p>Group hosts according to the topology, for example: db, web, leaf, spine.</p>
</dd>
<dt>Where</dt><dd><p>Group hosts by geographic location, for example: datacenter, region, floor, building.</p>
</dd>
<dt>When</dt><dd><p>Group hosts by stage, for example: development, test, staging, production.</p>
</dd>
</dl>
</li>
</ul>

<section id="use-metagroups">
<h3>Use metagroups</h3>
<p>Create a

 metagroup that organizes multiple groups in your inventory with the following syntax:</p>
<div class="highlight-yaml notranslate"><div class="highlight"><pre><span></span><span class="nt">metagroupname</span><span class="p">:</span>
<span class="w">  </span><span class="nt">children</span><span class="p">:</span>
</pre></div>
</div>
<p>The following inventory illustrates a basic structure for a data center. This example inventory contains a <code class="docutils literal notranslate"><span class="pre">network</span></code> metagroup that includes all network devices and a <code class="docutils literal notranslate"><span class="pre">datacenter</span></code> metagroup that includes the <code class="docutils literal notranslate"><span class="pre">network</span></code> group and all webservers.</p>
<div class="highlight-yaml notranslate"><div class="highlight"><pre><span></span><span class="nt">leafs</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">leaf01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.100</span>
<span class="w">    </span><span class="nt">leaf02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.110</span>

<span class="nt">spines</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">spine01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.120</span>
<span class="w">    </span><span class="nt">spine02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.130</span>

<span class="nt">network</span><span class="p">:</span>
<span class="w">  </span><span class="nt">children</span><span class="p">:</span>
<span class="w">    </span><span class="nt">leafs</span><span class="p">:</span>
<span class="w">    </span><span class="nt">spines</span><span class="p">:</span>

<span class="nt">webservers</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">webserver01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.140</span>
<span class="w">    </span><span class="nt">webserver02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.150</span>

<span class="nt">datacenter</span><span class="p">:</span>
<span class="w">  </span><span class="nt">children</span><span class="p">:</span>
<span class="w">    </span><span class="nt">network</span><span class="p">:</span>
<span class="w">    </span><span class="nt">webservers</span><span class="p">:</span>
</pre></div>
</div>
</section>

<section id="create-variables">
<h3>Create variables</h3>
<p>Variables set values for managed nodes, such as the IP address, FQDN, operating system, and SSH user, so you do not need to pass them when running Ansible commands.</p>
<p>Variables can apply to specific hosts.</p>
<div class="highlight-yaml notranslate"><div class="highlight"><pre><span></span><span class="nt">webservers</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">webserver01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.140</span>
<span class="w">      </span><span class="nt">http_port</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">80</span>
<span class="w">    </span><span class="nt">webserver02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.150</span>
<span class="w">      </span><span class="nt">http_port</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">443</span>
</pre></div>
</div>
<p>Variables can also apply to all hosts in a group.</p>
<div class="highlight-yaml notranslate"><div class="highlight"><pre><span></span><span class="nt">webservers</span><span class="p">:</span>
<span class="w">  </span><span class="nt">hosts</span><span class="p">:</span>
<span class="w">    </span><span class="nt">webserver01</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.140</span>
<span class="w">      </span><span class="nt">http_port</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">80</span>
<span class="w">    </span><span class="nt">webserver02</span><span class="p">:</span>
<span class="w">      </span><span class="nt">ansible_host</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">192.0.2.150</span>
<span class="w">      </span><span class="nt">http_port</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">443</span>
<span class="w">  </span><span class="nt">vars</span><span class="p">:</span>
<span class="w">    </span><span class="nt">ansible_user</span><span class="p">:</span><span class="w"> </span><span class="l l-Scalar l-Scalar-Plain">my_server_user</span>
</pre></div>
</div>
</section>
