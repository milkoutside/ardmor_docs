# Jobber Bot 
## Расположение Квот


Квоты находятся в Ardmor QMS ->  Quote Report

![image](https://github.com/milkoutside/google_widget/assets/71405386/ec979b93-0ded-4728-8ded-d88779c2b4c8)

![image](https://github.com/milkoutside/google_widget/assets/71405386/b5ab4c91-cc8a-4956-b7f4-33d8985c0413)

Jobber

![image](https://github.com/milkoutside/google_widget/assets/71405386/67ce3887-3b4b-44c7-b733-7ba490f582ca)

## Отправка с Creator на Server Jobber Bot

У каждой Quote есть кнопкa “SendToJobber”, которая при нажатии отправляет json-данные на Zoho Flow, а та в свою очередь на сервер.

Функция в Creator находится в Edit the application -> Workflows -> Functions ->sentToJobber. В функции есть 3 части 1 – создание запись в QSM Jobber Helper Report

![image](https://github.com/milkoutside/google_widget/assets/71405386/2052782e-130e-4eaa-bc07-ff632156db84)

В QSM Jobber Helper Report поле DATA MANAGER хранит статус переноса в Jobber

<pre>ReportedQuoteID = insert into QSM_Jobber_Helper
	[
		Added_User=zoho.loginuser
		Jobber_Property_ID=JobberPropertyID
		JOB_TYPE=qt_obj.JOB_TYPE.JOB_TYPE
		MATERIALS_TOTAL_COST=qt_obj.MATERIALS_TOTAL_Quote_COST
		COMPANY_NAME="ARDMOR WINDOWS & DOORS, INC"
		DATA_MANAGER="Sent to Bot (Jobber)"
		Quote_ID=quoteId
		REMEASUREMENT=trimmedRemesurement
		OFFICE_NOTE=qt_obj.OFFICE_NOTE
		NOTE_FOR_TECHNICIAN=qt_obj.NOTE_FOR_TECHNICIAN
		Client_Info=qt_obj.Client_Info
		TIME_FOR_JOB=qt_obj.TIME_FOR_JOB_ALL
		JOBBER_QUOTE=qt_obj.Quote_No
		WORKERS=qt_obj.WORKERS
		JOB_CAN_BE_SCHEDULED_FOR_VISIT=qt_obj.JOB_CAN_BE_SCHEDULED_FOR_VISIT
		JC_INFO=qt_obj.JOB_COST_INFO
		TOTAL_JC=qt_obj.JOB_COST_TOTAL
		Client_Name=FullName
		JOBBER_CLIENT_LINK=CustomerJobberLink
		Estimator_Email=EstimatorEmail
		Estimator_Phone=EstimatorPhone
		ESTIMATOR=EstimatorName
		HOA_HISTORIC_PERMIT=HOAHISTORICPERMIT
		Subtotal=qt_obj.SUBTOTAL
		Discount=qt_obj.Apply_Discount
		Discount_Amount=qt_obj.Discount
		PREFERRED_METHOD_OF_PAYMENT=qt_obj.METHOD_OF_PAYMENT
		Customer_Birthday=qt_obj.Customer_Birthday
		TOTAL=qt_obj.TOTAL
		REQUIRED_DEPOSIT=qt_obj.REQUIRED_DEPOSIT
		QUOTE_EXPIRATION_DATE=quoteExpirationDate
		Pagename=qt_obj.Pagename
		ATTACHED_FILE_1=qt_obj.STORM_DOORS_FILE
		ATTACHED_FILE_2=qt_obj.MARVIN_WINDOWS_DOORS_FILE
		ATTACHED_FILE_3=qt_obj.ANDERSEN_WINDOWS_DOORS_FILE
		ATTACHED_FILE_4=qt_obj.PELLA_WINDOWS_DOORS_FILE
		ATTACHED_FILE_5=qt_obj.OTHER_WINDOWS_DOORS_FILE
		ATTACHED_FILE_6=qt_obj.ENTRY_DOORS_FILE
		ATTACHED_FILE_7=qt_obj.ATTACH_FILE_1
		ATTACHED_FILE_8=qt_obj.ATTACH_FILE_2
		ATTACHED_FILE_9=qt_obj.ATTACH_FILE_3
		ATTACHED_FILE_10=qt_obj.ATTACH_FILE_4
		ATTACHED_FILE_11=qt_obj.ATTACH_FILE_5
		ATTACHED_FILE_12=qt_obj.ATTACH_FILE_6
		ATTACHED_FILE_13=qt_obj.OKNOPLAST_WINDOWS_DOORS_FILE
		ATTACHED_FILE_14=qt_obj.Other_file_pdf
		ATTACHED_FILE_15=qt_obj.ATTACH_FILE_8
		ATTACHED_FILE_16=qt_obj.ATTACH_FILE_9
		SERVICE_TYPE=qt_obj.SERVICE_TYPE
		SALES_MANAGER_NOTE=qt_obj.Sales_Manager_Note
	];
	NewReportedQuote = QSM_Jobber_Helper[ID == ReportedQuoteID]; 
</pre>

2 Отправляет на почту письма

3 Отправляет данные на Zoho Flow

<pre>reportData = Map();
	reportData.put('Added_User',ifNull(zoho.loginuser,""));
	reportData.put('Jobber_Property_ID',ifNull(JobberPropertyID,""));
	reportData.put('JOB_TYPE',ifNull(qt_obj.JOB_TYPE_V2.JOB_TYPE,""));
	reportData.put('Customer_Name',ifNull(FullName,""));
	reportData.put('MATERIALS_TOTAL_COST',ifNull(qt_obj.MATERIALS_TOTAL_Quote_COST,""));
	reportData.put('COMPANY_NAME',ifNull("ARDMOR WINDOWS & DOORS, INC",""));
	reportData.put('DATA_MANAGER',ifNull("Sent to Bot (Jobber)",""));
	reportData.put('Quote_ID',ifNull(quoteId,""));
	reportData.put('REMEASUREMENT',ifNull(trimmedRemesurement,""));
	reportData.put('OFFICE_NOTE',ifNull(qt_obj.OFFICE_NOTE,""));
	reportData.put('NOTE_FOR_TECHNICIAN',ifNull(qt_obj.NOTE_FOR_TECHNICIAN,""));
	reportData.put('Client_Info',ifNull(qt_obj.Client_Info,""));
	reportData.put('TIME_FOR_JOB',ifNull(qt_obj.TIME_FOR_JOB_ALL,""));
	reportData.put('JOBBER_QUOTE',ifNull(qt_obj.Quote_No,""));
	reportData.put('WORKERS',ifNull(qt_obj.WORKERS,""));
	reportData.put('JOB_CAN_BE_SCHEDULED_FOR_VISIT',ifNull(qt_obj.JOB_CAN_BE_SCHEDULED_FOR_VISIT,""));
	reportData.put('JC_INFO',ifNull(qt_obj.JOB_COST_INFO,""));
	reportData.put('TOTAL_JC',ifNull(qt_obj.JOB_COST_TOTAL,""));
	reportData.put('Client_Name',ifNull(FullName,""));
	reportData.put('JOBBER_CLIENT_LINK',ifNull(CustomerJobberLink,""));
	reportData.put('Estimator_Email',ifNull(EstimatorEmail,""));
	reportData.put('Estimator_Phone',ifNull(EstimatorPhone,""));
	reportData.put('ESTIMATOR',ifNull(EstimatorName,""));
	reportData.put('HOA_HISTORIC_PERMIT',ifNull(HOAHISTORICPERMIT,""));
	reportData.put('Subtotal',ifNull(qt_obj.SUBTOTAL,""));
	reportData.put('Discount',ifNull(qt_obj.Apply_Discount,""));
	reportData.put('Discount_Amount',ifNull(qt_obj.Discount,""));
	reportData.put('PREFERRED_METHOD_OF_PAYMENT',ifNull(qt_obj.METHOD_OF_PAYMENT,""));
	reportData.put('Customer_Birthday',ifNull(qt_obj.Customer_Birthday,""));
	reportData.put('TOTAL',ifNull(qt_obj.TOTAL,""));
	reportData.put('REQUIRED_DEPOSIT',ifNull(qt_obj.REQUIRED_DEPOSIT,""));
	reportData.put('Client_ID',ifNull(JobberID,""));
	reportData.put('QUOTE_EXPIRATION_DATE',ifNull(quoteExpirationDate,""));
	reportData.put('Pagename',ifNull(qt_obj.Pagename,""));
	reportData.put('ATTACH_FILE_1',ifNull(qt_obj.ATTACH_FILE_1,""));
	reportData.put('ATTACH_FILE_2',ifNull(qt_obj.ATTACH_FILE_2,""));
	reportData.put('ATTACH_FILE_3',ifNull(qt_obj.ATTACH_FILE_3,""));
	reportData.put('ATTACH_FILE_4',ifNull(qt_obj.ATTACH_FILE_4,""));
	reportData.put('ATTACH_FILE_5',ifNull(qt_obj.ATTACH_FILE_5,""));
	reportData.put('ATTACH_FILE_6',ifNull(qt_obj.ATTACH_FILE_6,""));
	reportData.put('ATTACH_FILE_8',ifNull(qt_obj.ATTACH_FILE_8,""));
	reportData.put('ATTACH_FILE_9',ifNull(qt_obj.ATTACH_FILE_9,""));
	reportData.put('ATTACH_FILE_10',ifNull(qt_obj.STORM_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_11',ifNull(qt_obj.MARVIN_WINDOWS_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_12',ifNull(qt_obj.ANDERSEN_WINDOWS_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_13',ifNull(qt_obj.PELLA_WINDOWS_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_14',ifNull(qt_obj.OTHER_WINDOWS_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_15',ifNull(qt_obj.ENTRY_DOORS_FILE,""));
	reportData.put('ATTACH_FILE_16',ifNull(qt_obj.Other_file_pdf,""));
	reportData.put('ATTACH_FILE_7',ifNull(qt_obj.OKNOPLAST_WINDOWS_DOORS_FILE,""));
	reportData.put('SERVICE_TYPE',ifNull(qt_obj.SERVICE_TYPE,""));
	reportData.put('SALES_MANAGER_NOTE',ifNull(qt_obj.Sales_Manager_Note,""));
	reportData.put('QUOTE_TYPE',ifNull(qt_obj.QUOTE_TYPE,""));
	invokeurl
	[
		url :"https://flow.zoho.com/720277342/flow/webhook/incoming?zapikey=1001.9482f65e2046d5d77e272391714a4429.1fe463955948d80a8263d2a874f4ee42&isdebug=false"
		type :POST
		parameters:{"ReportedQuote":ReportedQuoteID,"Fields":reportData,"Items":li,"Report_Id":quoteId}
	]
</pre>

В Zoho Flow “GetJobberExecute” отправляет данные на сервер обычным invoke url.

![image](https://github.com/milkoutside/google_widget/assets/71405386/9fd07aec-5a14-4ef2-935a-8cde1a34b491)

## Jobber Bot Server

Из Zoho Flow данные отправляются на [JobberBotController.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Controllers/JobberBotController.php)

Далее мы создаем или редактируем существующую квоту.

Если данных нет в строке <b>JOBBER_QUOTE</b>(номера квоты), то это значит, что мы должны создать новую.

<pre>
	if ($fields['JOBBER_QUOTE'] == "" && strlen($fields['JOBBER_QUOTE'] <= 0)) {
                    echo "1\n";

                    dispatch(new CreateQuoteJob($creatorData, $creatorData['ReportedQuote']));

                } </pre>
		
Если же номер квоты есть, то редактируем

<pre>else {
                    echo "2\n";
                    $queryQuote = new QuoteSearchTerm();

                    $queryQuote->setTerm($fields['JOBBER_QUOTE']);
                    $queryResponse = $queryQuote->setQuery()->setHeaders()->get();

                    //check quote
                    if ((isset($queryResponse['data']['quotes']['nodes'][0]['quoteNumber']) == $fields['JOBBER_QUOTE'])) {
                        dispatch(new EditQuoteJob2($creatorData, $queryResponse['data']['quotes']['nodes'][0]['id'], $creatorData['ReportedQuote'],$fields['JOBBER_QUOTE']));


                    }</pre>

## Логирование ошибок

Ошибки обрабатываются в каждой job. Успешно или нет отправляется в Creator->Ardmor Qms-> QSM Jobber Helper Report

<img width="958" alt="еуіе" src="https://github.com/milkoutside/google_widget/assets/71405386/3d251757-9a96-4eeb-bc86-f5ddf4286298">

## Создание Квоты И Редактирование

Первым делом создается Квота [CreateQuoteJob.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Jobs/CreateQuoteJob.php)

Сначала проверяется, что она вообще существует

<pre>         $queryQuote->setQuoteId(base64_encode("gid://Jobber/Quote/" . $fields['JOBBER_QUOTE']));
            $queryResponse = $queryQuote->setQuery()->setHeaders()->get();
            if (!isset($queryResponse['data']['quote']['id'])) {</pre>

А дальше уже создаем саму квоту

<pre>
	 $createQuote = new CreateQuote();
                $createQuote->setHeaders();
                $createQuote->setPropertyId(base64_encode("gid://Jobber/Property/" . $fields['Jobber_Property_ID']));
                $createQuote->setClientId(base64_encode("gid://Jobber/Client/" . $fields['Client_ID']));
                $plainText = $this->formatHtml($items[0]);

                $createQuote->addLineItem([
                        'name' => $items[0]['name'],
                        'description' => $plainText,
                        'unitPrice' => 0.00,
                        'quantity' => $item['qty'] ?? 1,
                        'saveToProductsAndServices' => false
                    ])
                    ->addCustomField($customFields)
                    ->setDiscount(0.0)
                    ->setDeposit(0.0);
                $createQuote->setTitle($fields["JOB_TYPE"]);

                $createQuote->setQuery();
                $rr = $createQuote->get();
                Log::info("r of jobb");
                Log::info(json_encode($rr));

                $responseCreatedQuote = $rr['data']['quoteCreate']['quote'];

                $quoteId = $responseCreatedQuote['id'];
                $quoteNumber = $responseCreatedQuote['quoteNumber'];

</pre>

### Примечание

1. Estimated Price Appointment - поле, которое должно идти всегда первым, если оно существует в квоте
2. Есть два типа полей: 1. - стандартные ноды - продукты без прайса и количества с картинкой. 2. Продукты с прайсом и картинкой - LineItems

   Стандартная нода

   ![image](https://github.com/milkoutside/google_widget/assets/71405386/02d9232e-301b-4295-bbc6-41a3e95e7977)

   LineItem
   
   ![image](https://github.com/milkoutside/google_widget/assets/71405386/736591f4-b08a-4587-8a18-6b3ed13741d4)

   Далее Вызывается [EditQuoteJob2.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Jobs/EditQuoteJob2.php), где проверяем наличие Estimated Price Appointment

   <pre>
	   
            $arrayEst = [];
            foreach ($nodes as $est) {
                if ($est["name"] === "Estimated Price Appointment:") {

                    $plainText = $this->formatHtml($est);

                    $arrayEst['description'] = $plainText;
                    $arrayEst['name'] = $est["name"];
                    $haveEst = true;

                }

            }
   </pre>

   Скачиваем вложенные документы и бросаем их в квоту

   <pre>$downloadImages = [];
            $current_date = date("Ymd");
            $current_time = date("His");
            $current_datetime = $current_date . $current_time;
            $nodesHelper = new NodesHelper();
            $creator = new DownloadFile();
            $downloadImages = [];
            $current_date = date("Ymd");
            $current_time = date("His");
            $current_datetime = $current_date . $current_time;
            // Пример обработки поля 'Fields'
            $quotePrepareCustomFields = new QuotePrepareCustomFields();
            $customFields = $quotePrepareCustomFields->prepareCustomFields($fields);
            $creator = new DownloadFile();
            $attachmentsUrl = [];
            $deleteItemsAttachments = [];
            $nodesHelper = new NodesHelper();
            for ($i = 0; $i <= 16; $i++) {
                if (isset($fields['ATTACH_FILE_' . $i]) && $fields['ATTACH_FILE_' . $i] !== "") {
                    $downloadUrl = match ($i) {
                        7 => "OKNOPLAST_WINDOWS_DOORS_FILE",
                        10 => "STORM_DOORS_FILE",
                        11 => "MARVIN_WINDOWS_DOORS_FILE",
                        12 => "ANDERSEN_WINDOWS_DOORS_FILE",
                        13 => "PELLA_WINDOWS_DOORS_FILE",
                        14 => "OTHER_WINDOWS_DOORS_FILE",
                        15 => "ENTRY_DOORS_FILE",
                        16 => "Other_file_pdf",
                        default => "ATTACH_FILE_" . $i,
                    };
                    $responseDownload = $creator->setHeaders()->setField($downloadUrl. "/download")
                        ->setRecordId($fields['Quote_ID'])->setForm("Quote1_Report")->setUrl()->get();
                    if ($responseDownload->successful()) {
                        $content = $responseDownload->body();
                        $filename = $fields['ATTACH_FILE_' . $i];
                        $path = public_path('attachments') . '/' . $filename;
                        file_put_contents($path, $content);
                        $deleteItemsAttachments[] = $path;
                        $attachmentsUrl[] = "https://zoho.ardmor.com/attachments/" . $filename;
                    }
                }
            }
            $records = new QuoteCreateNote();
            $records->setHeaders();
            foreach ($attachmentsUrl as $attachment) {
                $records->setQuoteId($this->quoteId);
                $records->setAttachments(['url' => $attachment]);
                $records->setQuery();
            }
            $records->get();</pre>

   Далее мы должны скачать картинки из продуктов, чтобы в дальнейшем вытянуть из них s3key. В продукты картинка бросается не url, а именно s3key! 

   <pre>
	    $downloadImages = [];
            $i = 0;
            $textNodesItems = [];
            $downloadImages = [];


            foreach ($items as $index=>$field) {


                if ($nodesHelper->GetFileName($field['name']) != "none") {
                    $filename = $nodesHelper->GetFileName($field['name']);
                    $fileExtension = $nodesHelper->GetFileExtension($field['name']);
                    // $downloadImages[] = ['url' => 'https://zoho.ardmor.com/images/PAYMENT_SCHEDULE/' . $filename, 'ext' => $fileExtension, 'fileName' => $filename, 'index' => $i];
                    $textNodesItems[] = ['url' => $nodesHelper->GetFilePath($field['name']), 'ext' => $fileExtension, 'fileName' => $filename, 'index' => $index];
                    $items[$index]['read'] = true ;

                }
                elseif ($field['photo'] !== "" && isset($field['photo'])) {

                    $responseDownload = $creator->setHeaders()->setField("Photo/download")
                        ->setRecordId($field['id'])->setForm("All_Services_Report")->setUrl()->get();

                    if ($responseDownload->successful()) {
                        $content = $responseDownload->body();

                        $filename = $index . $fields['Client_ID'] . ".jpg";
                        $path = public_path('attachments') . '/' . $filename;
                        $fileExtension = pathinfo($filename, PATHINFO_EXTENSION);
                        file_put_contents($path, $content);

                        $downloadImages[] = ['url' => 'https://zoho.ardmor.com/attachments/' . $filename, 'ext' => $fileExtension, 'fileName' => $filename, 'index' => $index,'patch'=>$path];
                        $items[$index]['read'] = false ;

                    }
                }
                else{
                    $items[$index]['read'] = false ;
                }

            }
   </pre>

   ### Примечание

   Так как у нас нет хранилища амазон, то мы забрасывает картинки продуктов в Jobs запись "Dump Bot". Когда картинки бросаются во вложения, то они автоматически добавляются в хранилище амазон. Соответсвенно, мы можем потом вытянуть ключ. Бросаем в Job только с той целью, что в Quotes нет метода на удаление вложений, так что хламить мусором среди основных документов отказались, ибо квота может редактироваться много раз.
	Сохраняем в notes Jobs картинки
 <pre>           $deleteNotesJobs[] = [];


            foreach ($downloadImages as $url) {


                $records = new JobsCreateNote();
                $records->setHeaders();
                $records->setAttachments(['url' => $url['url']]);
                $r = $records->setQuery()->get();


                $deleteNotesJobs[] = [$url['fileName']];

            }
            foreach ($textNodesItems as $url) {

                $records = new JobsCreateNote();
                $records->setHeaders();
                $records->setAttachments(['url' => $url['url']]);
                $r = $records->setQuery()->get();


                $deleteNotesJobs[] = [$url['fileName']];

            }
            $quoteRequest = new QueryQuote();
            $quoteRequest->setQuoteId($this->quoteId);
            $quoteRequest->setQuery()->setHeaders();
            $quoteData = $quoteRequest->get();

            $s3keyImages = [];
 </pre>

После этого берем все вложения обратно и проверяем их ссылки на совпадение имен с картинками, которые мы качали

<pre>
	       $searchNotes = new JobsSearchNotes2();
            $searchNotes->setHeaders();
            $hasMoore = true;
            $cursor = null;
            $nodesS3 = [];
            while ($hasMoore)
            {
                $searchNotes->setCursor($cursor);
                $searchNotes->setQuery();
                $response = $searchNotes->get();
                $cursor = $response['data']['job']['notes']['pageInfo']['endCursor'];

                foreach ($response['data']['job']['notes']['nodes'] as $node) {
                    $fileAttachments = $node['fileAttachments']['nodes'];
                    $id = $node['id'];

                    if (!empty($fileAttachments)) {
                        foreach ($fileAttachments as $attachment) {
                            if (isset($attachment['fileName'])) {
                                // Проверяем наличие ключа "fileName" в элементе $attachment
                                $nodesS3[] = $attachment;

                            }

                        }
                    }
                    $deleteNotes[] = [$id];

                    // Добавляем "id" в массив $deleteNotes
                }
                if (!$response['data']['job']['notes']['pageInfo']['hasNextPage']) {
                    $hasMoore  = false;
                }
            }



            foreach ($downloadImages as $index=>$node) {


                foreach ($nodesS3 as $nameImage)
                {

                    if($node['fileName'] == $nameImage['fileName'])
                    {
                        $pattern = "/note_file_attachments\/[A-Za-z0-9\-]+\/original\/[A-Za-z0-9_\-\.]+\.\w+/";
                        if (preg_match($pattern, $node['url'], $matches)) {

                            $s3keyImages[] = ['url' => $matches[0], 'fileName' => $node['fileName'],'read'=>false,'index'=> $node['index']];

                        }
                    }
                }

            }
            foreach ($nodesS3 as $node) {
                foreach ($textNodesItems as $nameImage)
                {
                    if($node['fileName'] == $nameImage['fileName'])
                    {
                        $pattern = "/note_file_attachments\/[A-Za-z0-9\-]+\/original\/[A-Za-z0-9_\-\.]+\.\w+/";
                        if (preg_match($pattern, $node['url'], $matches)) {

                            $s3keyImages[] = ['url' => $matches[0], 'fileName' => $node['fileName'],'read'=>true,'index'=>$nameImage['index']];

                        }
                    }
                }

            }
</pre>

Создаем продукты и удаляем вложения из модуля Jobs
<pre>
	 $searchQuote = new QueryQuote();
            $searchQuote->setQuoteId($this->quoteId);
            $searchQuote->setHeaders();
            $searchQuote->setQuery();
            $needDelete = $searchQuote->get()['data']['query']['lineItems']['nodes'];
            if($haveEst)
            {
                $createEstimator = new CreateQuoteTextLineItem();
                $esp =$createEstimator->setHeaders()->setQuoteId($this->quoteId)->addLineItem($arrayEst)->setQuery()->get();

            }
            for ($i = 0; $i < count($items); $i++) {

                $quoteCreateItem = new CreateQuoteLineItem();
                $quoteCreateItemText = new CreateQuoteTextLineItem();
                $quoteCreateItem->setQuoteId($this->quoteId)->setHeaders();
                $quoteCreateItemText->setQuoteId($this->quoteId)->setHeaders();

                $arrayLine = [];
                if(!$items[$i]['read']) {

                    $plainText = $this->formatHtml($items[$i]);

                    $arrayLine =
                        [
                            'name' => $items[$i]['name'],
                            'description' => $plainText,
                            'unitPrice' => $items[$i]['cost'],
                            'quantity' => $items[$i]['qty'] ?? 1,
                            'saveToProductsAndServices' => false
                        ];

                    $r = $quoteCreateItem->addLineItem($arrayLine)->setQuery()->get();

                }
                else{
                    $plainText = $this->formatHtml($items[$i]);

                    $arrayLine =
                        [
                            'name' => $items[$i]['name'],
                            'description' => $plainText,
                        ];
                    }
                   $quoteCreateItemText->addLineItem($arrayLine)->setQuery()->get();
                if($i == 0)
                {
                    foreach ($needDelete as $node)
                    {
                        $deleteJobberLine = new QuoteDeleteLineItems();
                        $deleteJobberLine->setHeaders();
                        $deleteJobberLine->setQuoteId($this->quoteId);
                        $deleteJobberLine->addLineItem($node['id']);
                        $r = $deleteJobberLine->setQuery()->get();

                    }
                }

                    }


            $editCurrentQuote = new EditeQuote();
            $editCurrentQuote->setQuoteId($this->quoteId);
            $editCurrentQuote->setHeaders();
            $editCurrentQuote->setDiscount($fields['Discount']);
            $editCurrentQuote->setDeposit($fields['REQUIRED_DEPOSIT']);
            $editCurrentQuote->setTitle($fields['JOB_TYPE']);
            $editCurrentQuote->addCustomField($customFields);
            $editCurrentQuote->setQuery()->get();
            foreach ($deleteItemsAttachments as $patch)
            {
                if (file_exists($patch)) {
                    unlink($patch);
                }
            }
</pre>

Далее вызываем [EditQuoteLineItemsJob2.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Jobs/EditQuoteLineItemsJob2.php) и берем s3keys, после чего уже вставляем картинки в продукты, добавляем скидку, депозит, когда у нас вставлены все проукты и их прайсы.

<pre>
	searchQuote->setQuoteId($this->quoteId);
            $searchQuote->setHeaders();
            $searchQuote->setQuery();
            $nodes = $searchQuote->get()['data']['query']['lineItems']['nodes'];

            $lineItemsIds = [];
            foreach ($nodes as $item) {
                if($item['textOnly'])
                {
                    $lineItemsIds[] = ['id'=>$item['id'],'read'=>true];
                }
                else{
                    $lineItemsIds[] = ['id'=>$item['id'],'read'=>false];
                }



            }
            $quoteEdit = new EditeQuoteLineItem();
            $quoteEdit->setQuoteId($this->quoteId);

            if($this->haveEst)
            {
                array_shift($lineItemsIds);

            }

            for ($i = 0; $i < count($lineItemsIds); $i++) {

                if(!$lineItemsIds[$i]['read']) {

                    $arrayLine = [];
                    $arrayLine['unitPrice'] = $this->items[$i]['cost'];
                    $arrayLine['lineItemId'] = $lineItemsIds[$i]['id'];
                    for ($k = 0; $k < count($this->downloadImages); $k++) {

                        if ($this->downloadImages[$k]['index'] == $i) {

                            for ($j = 0; $j < count($s3keyImages); $j++) {

                                if ($this->downloadImages[$k]['fileName'] == $s3keyImages[$j]['fileName']) {


                                    $arrayLine = array_merge([
                                        'lineItemId' => $lineItemsIds[$this->downloadImages[$k]['index']]['id'],
                                        'image' => [
                                            's3Key' => $s3keyImages[$j]['url'],
                                            'id' => $i . $lineItemsIds[$this->downloadImages[$k]['index']]['id'],
                                            'fileName' => $this->downloadImages[$k]['fileName'],
                                            'fileableType' => $this->downloadImages[$k]['ext'],
                                            'fileSize' => 10,
                                            'contentType' => "Image",
                                        ]
                                    ], $arrayLine);
                                    break;
                                }
                            }
                        }
                    }
                    $quoteEdit->setLineItems($arrayLine);
                }
                if($lineItemsIds[$i]['read']) {
                    $arrayLine = [];
                    $arrayLine['lineItemId'] = $lineItemsIds[$i]['id'];
                    for ($k = 0; $k < count($this->textNodesItems); $k++) {
                        if ($this->textNodesItems[$k]['index'] == $i) {
                            for ($j = 0; $j < count($s3keyImages); $j++) {


                                if ($this->textNodesItems[$k]['fileName'] == $s3keyImages[$j]['fileName']) {

                                    $arrayLine = array_merge([
                                        'lineItemId' => $lineItemsIds[$this->textNodesItems[$k]['index']]['id'],
                                        'image' => [
                                            's3Key' => $s3keyImages[$j]['url'],
                                            'id' => $i . $lineItemsIds[$this->textNodesItems[$k]['index']]['id'],
                                            'fileName' => $this->textNodesItems[$k]['fileName'],
                                            'fileableType' => $this->textNodesItems[$k]['ext'],
                                            'fileSize' => 10,
                                            'contentType' => "Image",
                                        ]
                                    ], $arrayLine);
                                }
                            }
                        }
                    }
                    $quoteEdit->setLineItems($arrayLine);
                }
            }

            $response = $quoteEdit->setQuery()->setHeaders()->get();


            $decodedString = base64_decode($this->quoteId);

// Разделение на части с использованием разделителя '/'
            $parts = explode('/', $decodedString);

// Получение числовой части (если она существует)
            $numericPart = end($parts);


            $editCurrentQuote = new EditeQuote();
            $editCurrentQuote->setQuoteId($this->quoteId);
            $editCurrentQuote->setHeaders();
            $editCurrentQuote->setDiscount((float)$this->fields['Discount']);
            $editCurrentQuote->setDeposit((float)$this->fields['REQUIRED_DEPOSIT']);
            $editCurrentQuote->setTitle($this->fields['JOB_TYPE']);
            $editCurrentQuote->addCustomField($this->customFields);
            $editCurrentQuote->setQuery()->get();

</pre>

### Примечание 
1.Второе редатирование нужно было для того, ибо Jobber не разрешал раньше создавать продукты сразу с картинками
2.Кастомные поля добавляются в этом классе [QuotePrepareCustomFields.php](https://bitbucket.org/genioteamerp/ardmor/src/master/jobber/CustomFields/Quote/QuotePrepareCustomFields.php). Нужно добавить айди кастомного поля, его значение
Вытянуть айди можно таким graphql:
<pre>
	{
customFieldConfigurations(first: 50) {
    nodes {
      ... on CustomFieldConfigurationArea {
        id
        name
        valueType
        appliesTo
        readOnly
      }
      ... on CustomFieldConfigurationDropdown {
        id
        name
        valueType
        appliesTo
        readOnly
      }
      ... on CustomFieldConfigurationLink {
        id
        name
        valueType
        appliesTo
        readOnly
      }
      ... on CustomFieldConfigurationNumeric {
        id
        name
        valueType
        appliesTo
        readOnly
      }
      ... on CustomFieldConfigurationText {
        id
        name
        valueType
        appliesTo
        readOnly
      }
      ... on CustomFieldConfigurationTrueFalse {
        id
        name
        valueType
        appliesTo
        readOnly
      }
    }
  }
}
</pre>
3. Все graphql для запросов и мутаций находятся тут: [Graphs](https://bitbucket.org/genioteamerp/ardmor/src/master/jobber/JobberApi/Requests/Graphs/). Добавлять к текущим можно в соответсвии документации, но не слишком много, а то там есть лимит на количество выводимой информации.

# Task Widget

Этот [виджет](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Livewire/TaskWidget.php) предназначен для выполнения определнных действий по очереди. 
весь порядок действий [тут](https://bitbucket.org/genioteamerp/ardmor/src/master/config/flow.php) 
<pre>
	//Название очереди glass,parts
</pre>

Для отображения используется [Livewire](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Livewire/TaskWidget.php) 

### Отмена сценария

<pre>
	 public function declineScenario($scenario_id, $task_id)
    {
        Scenario::query()->where('id', '=', $scenario_id)->delete();
        $old_task = TaskZoho::find($task_id);
        $old_task->updateInZoho(['status' => 'Completed']);
        $old_task->saveToDB();
        session()->flash('message', "Scenario with id - {$scenario_id} was successful deleted");
        return redirect(request()->header('Referer'));
    }

</pre>

### Переход на следующий этап 
<pre>
	    public function store(string $task_id, string $btn_name, string $scenario)
    {
        $current_scenario = Scenario::query()
            ->where('name', '=', $scenario)
            ->whereJsonDoesntContain("params->stage->completed", 'END')
            ->where('deal_id', '=', $this->deal_id)
            ->first();
        $flow_actions = config("flow.{$scenario}.{$btn_name}.action");
        $flow_buttons = config("flow.{$scenario}.{$btn_name}.button");
        $owner = in_array('CONFIRMED', $flow_buttons) ? $current_scenario->confirm_owner : $current_scenario->task_owner;
        if(!array_key_exists('skip', $flow_actions)) {

            if (array_key_exists('close_task', $flow_actions) && $task_id) {

                $old_task = TaskZoho::find($task_id);
                if ($old_task->hasCorrectId()) {
                    $old_task->updateInZoho(['status' => 'Completed']);
                    $old_task->saveToDB();
                    session()->flash('message', "Task {$old_task->subject} with ID - {$task_id} completed");
                }
            }

            if (array_key_exists('open_zoho_sign', $flow_actions)) {
                $this->emit('openZohoSignTab', $this->deal);
            }

            $new_task = null;
            if (array_key_exists('create_task', $flow_actions)) {
                $new_task = TaskZoho::createInZoho([
                    'owner' => $owner,
                    'subject' => config("flow.{$scenario}.{$btn_name}.action.create_task.task_name"),
                    'description' => 'J# ' . $this->deal['job_number'],
                    'who_id' => $this->deal['contact_id'],
                    'what_id' => $this->deal['id'],
                    'se_module' => 'Deals',
                    'due_date' => now()->addDay()->format('Y-m-d')
                ]);
                $new_task->saveToDB();
                session()->flash('message', "New task {$new_task->subject} with ID - {$new_task->id} created");
            }
            $scenario_params = $current_scenario->params;
            if (empty($flow_buttons)) {
                $scenario_params['owner'] = [
                    'name' => 'task',
                    'id' => $owner
                ];
                $scenario_params['stage'] = [
                    'active' => [
                        'name' => '',
                        'task_id' => '',
                        'task_name' => ''
                    ],
                    'completed' => array_merge($current_scenario->params['stage']['completed'], [$current_scenario->params['stage']['active']['name'], $btn_name, 'END'])
                ];
            } else {
                $scenario_params['owner'] = [
                    'name' => in_array('CONFIRMED', $flow_buttons) ? 'confirm' : 'task',
                    'id' => $owner
                ];
                $scenario_params['stage'] = [
                    'active' => [
                        'name' => $btn_name,
                        'task_id' => $new_task ? $new_task->id : '',
                        'task_name' => $new_task ? $new_task->subject : ''
                    ],
                    'completed' => array_merge($current_scenario->params['stage']['completed'], [$current_scenario->params['stage']['active']['name']])
                ];
            }
            if ($this->creator_id) {
                $scenario_params['creator_id'] = $this->creator_id;
                $this->creator_id = null;
            }


            $current_scenario->update([
                'params' => $scenario_params
            ]);

            $this->fetchScenarios();
        }else{
            $scenario_params = $current_scenario->params;
            $old_task = TaskZoho::find($task_id);
            $scenario_params['stage'] = [
                'active' => [
                    'name' => $btn_name,
                    'task_id' => $task_id ?? "",
                    'task_name' => $old_task->subject ?? ""
                ],
                'completed' => array_merge($current_scenario->params['stage']['completed'], [$current_scenario->params['stage']['active']['name']])
            ];
            $current_scenario->update([
                'params' => $scenario_params
            ]);

            $this->fetchScenarios();
        }
    }
</pre>

Обычно этот виджет открывает, закрывает таски, открывает модальные формы zoho creator для заполнения и сабмита формы.

# По пути [ContactController.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Controllers/ContactController.php) 
у нас создание клиента в Jobber из запроса crm https://crm.zoho.com/crm/org785397330/settings/workflow-webhooks/5404667000025917032
Тут ничего особенного маппинг данных на jobber, создание записи

# Jobber создание requests в Jobber
Создание request у клиента в Jobber из вебхука
https://crm.zoho.com/crm/org785397330/settings/workflow-webhooks/5404667000035202005
Тоже ничего необычного маппинг данных, проверка клиента 
[RequestController.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Controllers/RequestController.php) 

# Создание клиента в Creator
По пути app/Http/Controllers/JobberController.php
у нас создание клиента в Jobber из запроса crm https://crm.zoho.com/crm/org785397330/settings/workflow-webhooks/5404667000025917032
Тут ничего особенного маппинг данных на creator, создание записи
[JobberController.php](https://bitbucket.org/genioteamerp/ardmor/src/master/app/Http/Controllers/JobberController.php) 

#Schedules ZOHO CRM

1. "IM_create_task_if_no_tasks_exist" - Создание тасков для сделок, где статус у тасков есть - Not Started
2. "oz_deals_update_need_a_quote_task" - Создание тасков и подсчет их overdue для тасков -  Not Started
3. "oz_deals_past_due" - подсчет past due days для тасков

#Functions Zoho Crm
1. "IM_qmsResyncButton" - подсчет материалов из creator-> Materials Orders->All Orders
   ![image](https://github.com/milkoutside/google_widget/assets/71405386/eb962e8f-5ad2-4832-837b-8fdfbb9b58e6)

Подсчет материалов по айдишникам из функции. Если есть, то считаем total

   

