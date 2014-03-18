Здесь будет про построение запросов в xPDO (xPDOCriteria и xPDOQuery)

This can be done using the embedded array syntax:

<?php
$query->where(array(
    array(
        'a' => 1,
        'b' => 1
    ),
    array(
        'OR:c:=' => 1,
        'd' => 1
    ),
));

If you specify an OR on the first condition within a nested array of conditions, this is used as the conjunction between it and any others at that level. 
