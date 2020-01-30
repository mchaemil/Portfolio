
# [[스티비] 프론트엔드 엔지니어 프로젝트 설명서 안내](https://www.notion.so/e8d67949da3c4f1f971051716c1f6cb1)

## 목적

- 프로젝트 설명서는 스티비 프론트엔드 엔제니어 채용의 필수 제출 사항입니다.
- 지원자의 작업물(코드)과 작업 과정에 대한 설명으로 지원자의 개발 이해도와 프로젝트 참여도를 확인하기 위함입니다.
- 스티비 프론트엔드 엔지니어 채용에 대한 자세한 내용은 [채용공고](https://www.notion.so/stibee/4f94730c0e6b4c5aab0ca5eea918665b)를 참고해주세요.

## 들어가야 할 내용

- 참여했던 대표 프로젝트의 코드
  - 공개 가능한 범위로 제출해주세요. 민감한 내용은 지워주세요.
  - 프로젝트 전체가 아닌 모듈 단위, 기능 단위여도 무관합니다.
  - 설계가 핵심이라면 설계를 중심으로, 구현이 핵심이라면 구현 주심으로 작성해 주시면 됩니다.
  - 분량의 많고 적음은 상관 없습니다.
- 위 코드에 대한 부연 설명
  - 기획 의도부터 설계, 참여도, 구현시 중요하게 생각하신 내용 들을 기술해 주세요.
  - 이 코드가 작성되기 까지의 과정을 설명해 주시면 좋습니다.
  - 코드에 대한 설명보다는 본인의 고민을 적어주세요.
  - 예) 프로젝트에 sass를 사용한 이유, 컴포넌트를 이렇게 구성한 이유, 메소드를 이렇게 분리한 이유, 컴포넌트의 props를 이렇게 구성한 이유, 이 시점(lifecycle)에 이런 서버 요청을 보내는 이유, 변수명을 이렇게 지은 이유, array 대신 object를 사용한 이유, 이걸 상수로 작성한 이유 ...


## 형식

- 자유 형식
  - 모듈, 기능, 컴포넌트 등 어떤 단위여 좋고 분량에 제한이 없습니다.
  - 코드와 설명을 따로 문서로 작성하셔도 좋고, 코드 내 주석으로 작성하셔도 좋습니다.
  - 코드에 대한 설명을 따로 문서로 작성하시는 경우 되도록 PDF로 제출해 주세요.


## 작성 예시

```css
$bg : #F4F4F5;
$lg : #e6e6eb;
$dg : #606060;

$normal : #333;
$dark : #000;
$dim : #b4b4b4;
$dimmer : #8c8c8c;
$hl : #d9af0e;

/*
- style.scss는 variable, nesting, mixin 등의 코딩 편의와 확장성을 위해 sass로 작성했습니다. 
- 이 파일은 gulp로 컴파일 됩니다. 
- 주요 키컬러와 텍스트 컬러를 변수화하여 추후 변경사항이 있을 때 유연하게 대처하고 싶었습니다.
- (리팩토링) 회색조 컬러 배색이 상대적으로 단조롭과 변수명 관리가 어려워 넘버링이 필요할 것 같습니다.
- (리팩토링) 컬러 변수명에 대한 보다 자세한 설명이 있었으면 좋겠습니다.
*/
```

```typescript
function Category({ match }: RouteComponentProps<MatchParams>) {
	
	const defaultLimit:number = 5;
	const [offset, setOffset] = useState(0);
	const [limit, setLimit] = useState(defaultLimit);
	const [rows, setRows] = useState([] as object[]);
	const [sortBy, setSortBy] = useState('id');
	const [searchTarget, setSearchTarget] = useState('title');
	const [searchKeyword, setSearchKeyword] = useState('');
	const [category, setCategory] = useState({
		category_name: ''
	});
	const [total, setTotal] = useState(0);
	const [isLoading, setIsLoading] = useState(true);

	const sortByList = [
		{key: 'created_time', name: '최신순'},
		{key: 'subscriber_count', name: '구독자순'}
	];
	
	useEffect(() => {
		getList();
	}, [match, offset, sortBy, searchKeyword]);


	// methods
	const getList = async () => {
		let params = {
			limit: limit,
			offset: offset,
			sortBy: sortBy,
			searchTarget: searchTarget,
			searchKeyword: searchKeyword,
			tag: tag.join(',')
		}
		
		let endpoint = `/category/${match.params.categoryMid}?${qs.stringify(params)}`;		
		
		const result = await Http.get(endpoint);

		if (result.data.rows.length === 0) {
			Alert.error('더 이상 없습니다');
			return;
		}

		setTotal(result.data.count);
		setCategory(result.data.category);
		// setRows(result.data.rows);
		setRows(rows.concat(result.data.rows));
		setIsLoading(false);
	}
...
}

/*
- 목록의 기본 파라미터 offset, limit, total 등을 state로 관리하도록 했다. 
- 정렬의 선택지를 배열로 key, name으로 관리하도록 했다.
- getList 메소드에서 서버 요청을 실제 실행하기 전에 params라는 오브젝트를 만들고 이를 스트링으로 변환하는 과정을 거친다. 더 좋은 방법이 없을까?
- (리팩토링) async, await를 사용했지만 setTotal 등의 상태 변경은 더 유연한 방법이 필요할 것 같다. 
- (리팩토링) 페이지네이션 방식의 변경을 대응할 수 있는 유연함 필요
*/
```
