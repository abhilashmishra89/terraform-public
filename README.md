[my Bookmarklet](javascript: (() => {
    
    let output = [];
    document.getElementById("suppress_fastly_nodes").checked = true;
    document.getElementById("submit_filters").click();
    const targets = ['error_backends', 'error_responses', 'error_paths'];
    for (let target of targets) {
        let data = "";
        let table = document.getElementById(target);
        for (let line = 0; line < table.rows.length; line++) {
            if (line > 10) break;
            let col1_value = table.rows[line].cells[0].textContent.replace(/\s\(View\)$/, "") || "";
            let col2_value = (table.rows[line].cells.length > 1) ? table.rows[line].cells[1].textContent : "";
            if (line === 0) col1_value = col1_value.replace(/\sCopy$/, "");
            let rowData = `${(col2_value === "")? "" : col2_value + "\t"}${col1_value}`;
            data = `${(data === "")? "" : data + "\r\n"}${rowData}`
        }
        output.push(data);
    }
    let $temp = $("<textarea>");
    $("body").append($temp);
    $temp.val(output.join("\r\n\r\n")).select();
    document.execCommand("copy");
    $temp.remove();
    document.getElementById("suppress_fastly_nodes").checked = false;
    document.getElementById("submit_filters").click();
    alert('Copied!');
})();)
